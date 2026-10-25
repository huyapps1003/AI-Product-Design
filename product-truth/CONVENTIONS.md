# Product truth conventions

**Status:** Adopted with the folder structure, 25 Oct 2026 · **Owner:** Henry

**Tóm tắt (VI):** Quy ước chung cho mọi file trong `product-truth/`: mỗi phát biểu có trạng thái (Known / Assumed / Open) và nguồn trích dẫn; khoảng trống thành câu hỏi mở, không tự nghĩ ra giải pháp.

## 1. What product truth is

Product truth describes what Global Campus OS **is**, as far as it is known. It is the input the AI design workflow reads before it shapes a flow or generates a screen.

- **Research** (`../research/`) is evidence. It records findings and never overrides product truth.
- **Product truth** states what is known and cites the evidence for it.
- **Decisions** (`07-decisions/`) are how product truth changes.
- **Open questions** (`08-open-questions/`) hold everything that is not known. Product truth never fills a gap with an invented answer.

## 2. Every statement has a status

| Status | Meaning | How to cite it | Can a design treat it as settled? |
|---|---|---|---|
| **Known** | Stated by Henry or a stakeholder, written in a source document, or found by research | The source: `Henry, 25 Oct 2026`, `Architecture § Users and roles`, `BR-06`, `RES-F02`, `MC-09` | Yes. Keep the research confidence label when it matters (for example "public claim") |
| **Assumed** | A working design direction chosen so design can start | The decision: `DL-11 (Assumed)` | Yes for design, but expect change |
| **Open** | Unknown | The question: `OQ-14` | No. Keep the element generic and note the OQ ID |

Architecture v1 and the Employee Data Dictionary v1 are both marked *Proposed*. Treat their content as Known for design purposes, except items they list as open.

## 3. IDs

| Prefix | What | Defined in |
|---|---|---|
| `RES-F01`… | Research findings | `../research/2026-10-25-campus-os-foundation-research.md` |
| `MC-01`… | What varies across the group (research catalogue) | same research file, §04; mapped in [`03-domain/variation-catalogue.md`](./03-domain/variation-catalogue.md) |
| `DL-01`… | Decisions (Assumed, Confirmed, Superseded) | [`07-decisions/decision-log.md`](./07-decisions/decision-log.md) |
| `OQ-01`… | Open questions | [`08-open-questions/open-questions.md`](./08-open-questions/open-questions.md) |
| `BR-01`…, `Decision D01`… | Data Dictionary business rules and unresolved decisions | [`00-sources/…Employee_Data_Dictionary_v1.md`](./00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) |
| `E01`…, `A01`…, `R01`…, entity `D01`–`D03` | Data Dictionary entities (employee, access, reference, academic) | same file |
| `P1`–`P5` | Admin Platform personas | [`02-users/roles-personas.md`](./02-users/roles-personas.md) |
| `S1`–`S5`, `SCR-…` | Admin Platform scenarios and screens | [`06-experience/admin-platform/`](./06-experience/admin-platform/) |
| `G1`–`G16` | Gaps from the 18 Oct review (archived) | [`_archive/gap-analysis-2026-10-18.md`](./_archive/gap-analysis-2026-10-18.md) |

"Decision D01" (Data Dictionary open decision) is not "entity D01" (academic role allocation); DL-12 records the proposed rename.

## 4. File header

Every file starts with:

```
# Title
**Status:** <Known | Assumed | Proposed | Living>, <date> · **Owner:** <name>
**Cites:** <research IDs, source sections, DL IDs>
**Tóm tắt (VI):** <one or two sentences in Vietnamese>
```

## 5. Scope rules

- Campus OS is run by Global Schools Group (GSG) for **all GSG brands** (DL-22). GIIS is the brand product truth describes in most detail; label GIIS-specific facts as GIIS examples, not as the whole scope.
- SmartLearn is out of scope for Campus OS product truth. Don't use it as context.
- People in examples are synthetic. Real brand, campus and country names come from the research and the source documents.

## 6. Where things go

| You have | Put it in |
|---|---|
| A new fact with a source | The topic file in `01-context/` … `05-data-governance/`, with the citation |
| A gap or contradiction | A new `OQ-` row in `08-open-questions/` |
| A design choice that lets work start | A new `DL-` entry (Assumed) in `07-decisions/`, then the affected files |
| A new design slice (for example the Parent app) | A new folder `06-experience/<slice>/`, copied from [`_templates/design-slice/`](./_templates/design-slice/README.md) |
| A superseded review or analysis | `_archive/` |
| New research evidence | `../research/`, never here |

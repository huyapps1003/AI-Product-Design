# Data classification

**Status:** Tiers Known (Architecture v1); tier names Assumed (DL-11); children's data tier Open, 25 Oct 2026 · **Owner:** Henry
**Cites:** [Architecture v1 § Security, privacy and compliance](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#security-privacy-and-compliance) · DL-11, DL-19 · Data Dictionary Decision D13 · [rules-to-ui § Field visibility](../06-experience/admin-platform/rules-to-ui.md#field-visibility-decision-d13) · research §07, §10
**Tóm tắt (VI):** Ba mức nhạy cảm dữ liệu và cách hiển thị trong thiết kế. Mức của dữ liệu học tập của trẻ em còn là câu hỏi mở (OQ-13).

## Tiers

| Tier | Name (DL-11, Assumed) | What it covers (Architecture v1) | Rules (Architecture v1) |
|---|---|---|---|
| 1 | Internal | Directory data | Visible within scope |
| 2 | Confidential | Academic and finance data | Visible within scope and capability |
| 3 | Restricted | Identity documents, health, SEN and counselling, child protection cases | Encrypted at field level, visible only to named roles, every read logged; record-level ACL (DL-19) |

Field-level tiers for employee data are in the Data Dictionary (Decision D13) and their UI treatment (P-LOCK, masking) in [rules-to-ui](../06-experience/admin-platform/rules-to-ui.md#field-visibility-decision-d13).

## What the evidence says

| Finding | Effect on this file | Open |
|---|---|---|
| PDPC (28 Mar 2024) tells organisations to treat children's data as generally sensitive and to run impact assessments before launch | DL-11 puts students' academic data in Tier 2; research rates DL-11 "partial" | OQ-13 |
| DPDP core obligations (≈ May 2027) cover notices, consent, children's data, security and retention | Retention schedules per country are named in Architecture v1 but not defined | OQ-21 |
| PowerSchool breach (Dec 2024) exposed family and educator data through a support credential | Access to Tier 3 by platform support is not defined | OQ-15 |

## Rules for design and AI use

- Restricted looks restricted: Tier 3 fields carry the lock treatment and are masked by default for viewers without the right.
- Tier 3 data is never sent to an AI model; any AI feature keeps a human approver (Architecture § Security).
- Group analytics show de-identified, aggregated data unless the viewer has cell-level access (rbac-model Layer 6; OQ-08).

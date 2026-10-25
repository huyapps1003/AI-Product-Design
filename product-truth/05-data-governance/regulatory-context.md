# Regulatory context

**Status:** Singapore and India Known (secondary legal sources); other countries Open, 25 Oct 2026 · **Owner:** Henry
**Cites:** RES-F05 · MC-04, MC-05, MC-08, MC-09, MC-12 · research §07 · [Architecture v1 § Multi-campus and tenancy model](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#multi-campus-and-tenancy-model), [§ Security](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#security-privacy-and-compliance)
**Tóm tắt (VI):** Quy định về dữ liệu và cơ quan quản lý ở Singapore và Ấn Độ, theo mốc thời gian. 9 quốc gia còn lại của GSG chưa được nghiên cứu.

Summaries from secondary legal sources, not legal advice. GSG operates in 11 countries; only Singapore and India are covered.

## Timeline

| When | What | Detail |
|---|---|---|
| 28 Mar 2024 | PDPC children's guidelines (SG) | Treat children's data as sensitive; impact assessments before launch; teenagers can consent if they understand the notice, otherwise ask parents |
| Dec 2024 | PowerSchool breach | A compromised support credential exposed data about families and educators |
| 14 Nov 2025 | DPDP Rules notified (IN) | Data Protection Board set up; everything else starts in phases |
| 2026 board exams | APAAR for CBSE | Mandatory, but schools may enter "refused" (parent withheld consent, copy kept) or "NOGEN" |
| ≈ May 2027 | DPDP core obligations | Notices, consent, children's data, security, retention. Architecture v1 plans the India cell from June 2027 |

## By topic

| Topic | Singapore | India |
|---|---|---|
| Consent for children | A child is anyone up to 18; teenagers may consent if they understand the notice; schools may require parental consent at higher ages | Verifiable parental consent for under-18s (reliable ID or DigiLocker); some education and safety processing exempt |
| Breach notification | Notify affected children, and parents where possible | Notify affected people at once; initial notice to the Board without delay, full report within 72 hours |
| Notices and language | Plain, age-appropriate language | Notice in English or a scheduled Indian language on request (DPDP Act s.5(3)) |
| Evidence for regulators | EduTrust, ICA Student Pass cases, FPS | CBSE list of candidates, APAAR status, processing logs kept at least one year |
| Parties defined by law | Organisation / data intermediary | Data fiduciary / data processor ([data-controller model](./data-controller-model.md)) |
| Penalties | Financial penalties under PDPA | Up to INR 2.5 billion per breach of obligations |

## Regulators and packs in product truth

| Country | Regulators and boards | Regulator pack (Architecture v1) |
|---|---|---|
| Singapore | CPE/SSG (EduTrust), ICA (Student Pass), PDPC | ICA Student Pass tracking, EduTrust reports, Fee Protection Scheme, medical insurance records |
| India | CBSE (affiliation, APAAR, list of candidates), Data Protection Board | CBSE affiliation data and board registration exports |
| Curriculum boards | IB, Cambridge | Candidate registration, predicted grades, IA and coursework (Architecture § Integrations) |

## Not researched

| Gap | Open |
|---|---|
| Residency and cross-border rules in Malaysia, Japan, Thailand, UAE, Vietnam, Saudi Arabia, South Korea, Cambodia, the Philippines, the UK | OQ-21 |
| Regulator reports and identifiers outside SG and IN | OQ-22 |
| Scope of the DPDP education exemptions | OQ-06 (legal review) |
| What the consent record holds | OQ-10 |
| Notice and UI languages | OQ-25 |

## Product truth that relates to these rules

- DL-07 (deny wins, fail closed) is consistent with PDPC guidance and DPDP penalties (research §10).
- DL-15 (English UI) is partial: DPDP gives parents a right to notices in scheduled Indian languages (OQ-25).
- Architecture v1: each country pack carries its retention schedule and consent forms; immutable audit log exportable for EduTrust and CBSE inspections.

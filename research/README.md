# Research

Evidence about the Global Campus OS problem space: users, the group, the market and regulation. AI-assisted design work reads it alongside `product-truth/`.

**Tóm tắt (VI):** Thư mục này chứa *bằng chứng nghiên cứu*, không phải product truth. Chỉ ghi phát hiện, nguồn, khoảng trống và câu hỏi mở; không đề xuất giải pháp.

## Rules

1. **Findings only.** Research files record what was found, where it came from and what is still unknown. They propose no solutions, decisions or architecture changes.
2. **Never overrides product truth.** If research and `product-truth/` disagree, product truth wins until a decision in `product-truth/06-decision-log.md` changes it. Research is the evidence a decision cites.
3. **Stable IDs.** Each file has an `id` in its front matter and numbered findings (`RES-F01`…) and catalogue rows (`MC-01`…). Cite them from decision-log entries, flows or screen specs, for example "Evidence: RES-F06, MC-02".
4. **Dated, not edited in place.** A new study is a new file named `YYYY-MM-DD-topic.md`. Correct mistakes in place, but put new evidence in a new file that links to the old one.
5. **Confidence is explicit.** Every source is labelled: internal primary, our own assumptions, public claims, vendor marketing, or secondary legal.

## How the AI design workflow uses it

| Step | Read | Use research for |
|---|---|---|
| Shaping product truth (briefs, personas, scenarios, decisions) | Relevant research file first, then `product-truth/` | Grounding the problem; citing evidence in DL entries |
| Generating a screen | `product-truth/` as today (see `prompts/system.md`) | Realistic context only. Don't design gaps listed here as if they were features |
| Reviewing a design | The research file's "Product truth vs evidence" and open questions | Spotting assumptions that the evidence doesn't support |

## Index

| File | Covers | Status |
|---|---|---|
| [2026-10-25-campus-os-foundation-research.md](./2026-10-25-campus-os-foundation-research.md) | All GSG brands: users and stakeholders, what varies across the group (MC-01–MC-15), core domains, market and competitors, regulation (PDPA, DPDP, APAAR), access control and data controller findings, product truth vs evidence, open questions | v3, 25 Oct 2026 · [published view](https://claude.ai/artifact/TvZwexKQU4Kj5HqhYyRbcG) |

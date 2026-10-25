# Experience: design slices

**Status:** Living, 25 Oct 2026 · **Owner:** Henry
**Cites:** DL-01 · [Architecture v1 § Domains and modules](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#domains-and-modules)
**Tóm tắt (VI):** Mỗi "design slice" là một phần sản phẩm đủ dữ liệu để thiết kế màn hình: brief, kịch bản, bản đồ màn hình, flow và quy tắc → UI. Hiện có một slice: Admin Platform.

A design slice is a part of the product with enough product truth to wireframe from. Each slice has the same five parts, so the AI design workflow can read any slice the same way.

| Part | File | Answers |
|---|---|---|
| Brief | `README.md` | Problem, scope, non-goals, success metrics, principles, canonical sample data |
| Scenarios | `scenarios.md` | The end-to-end jobs the slice must support |
| Screen map | `ia-screen-map.md` | Shell, navigation, screen list with IDs (`SCR-…`), personas, entities, MinV shells and blocks |
| Flows | `flows/` | Per scenario: user flow, state machine, screen states |
| Rules to UI | `rules-to-ui.md` | Business rules and decisions → UI behaviour, status vocabulary, field visibility |

## Slices

| Slice | Surface | Status | Folder |
|---|---|---|---|
| **Admin Platform** (Staff and HR, Global Admin, Team Management, Governance, self-service) | Staff app and web | Designed against Assumed decisions (DL-01) | [`admin-platform/`](./admin-platform/README.md) |
| Parent app | Parent app | Not started; no data dictionary | — |
| Student app | Student app | Not started | — |
| Leadership views | Leadership views | Not started | — |
| Campus modules (11 others) | Staff app and web | Stubs in the Admin Platform screen map | — |

To start a slice, copy [`../_templates/design-slice/`](../_templates/design-slice/README.md) into a new folder here. Read [`../01-context/`](../01-context/product-brief.md) to [`../05-data-governance/`](../05-data-governance/data-controller-model.md) first, and record every gap as an OQ instead of filling it.

## How a screen is designed from a slice

1. Pick the screen's ID in `ia-screen-map.md`.
2. Follow its scenario flow and screen states in `flows/`.
3. Apply the rules listed for it in `rules-to-ui.md` (status colours, field visibility).
4. Use the sample data in the slice `README.md`.
5. Check [`../03-domain/variation-catalogue.md`](../03-domain/variation-catalogue.md) for anything that varies by brand, country or campus, and [`../08-open-questions/`](../08-open-questions/open-questions.md) for anything still open.
6. Generate with `prompts/system.md` and the MinV tokens.

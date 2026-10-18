# Product truth

Canonical, versioned descriptions of what the Global Campus OS is. The design system (`design-tokens/`, `prompts/`) defines how screens look; these files define what the screens are about. AI-assisted design work should read the relevant file here before generating a screen.

| Document | Covers | Status |
|---|---|---|
| [Global Campus OS — Product Architecture v1.0](./Global_Campus_OS_Product_Architecture_v1.md) | Apps, 12 campus modules, shared core, users and roles, key workflows, tenancy, data model, tech stack, integrations, security, roadmap | Proposed |
| [Global Campus OS — Employee Data Dictionary v1.0](./Global_Campus_OS_Employee_Data_Dictionary_v1.md) ([xlsx](./Global_Campus_OS_Employee_Data_Dictionary_v1.xlsx)) | Employee master data and GA/TM access management | Proposed |

Start with the Product Architecture; module- or domain-level specs like the Employee Data Dictionary refine it.

## Design-ready layer (Admin Platform, v1 design slice)

These files turn the architecture and data dictionary into something a designer can wireframe from. Read them in order; each screen in `03` links back to a scenario (`02`, `04-flows/`) and to the rules it must show (`05`).

| File | Answers |
|---|---|
| [00-product-brief.md](./00-product-brief.md) | Problem, vision, scope, non-goals, success metrics, canonical sample data |
| [01-roles-personas.md](./01-roles-personas.md) | HR Admin, Global Admin, Campus Leader, Module Admin, Teacher, mapped to GA/TM |
| [02-key-scenarios.md](./02-key-scenarios.md) | 5 key scenarios |
| [03-ia-module-map.md](./03-ia-module-map.md) | Shell, module catalog (R01), navigation, screen list (SCR-…) |
| [04-flows/](./04-flows/) | User flow, state machine and screen states per scenario |
| [05-rules-to-ui.md](./05-rules-to-ui.md) | BR-01…BR-21 and Decisions D01…D15 → UI behaviour; status colours; field visibility |
| [06-decision-log.md](./06-decision-log.md) | Assumptions chosen and why |
| `07-rbac-model.md` | Effective RBAC model: six-layer access control for Campus OS (DL-16…DL-21) |
| [gap-analysis-2026-10-18.md](./gap-analysis-2026-10-18.md) | Review of v1 product truth that led to this layer |

**To design a screen:** pick its ID in `03`, read its scenario flow in `04-flows/`, apply the rules listed for it in `05`, and use the sample data in `00`.

# Product truth

Canonical, versioned description of what Global Campus OS is: the platform Global Schools Group (GSG) runs for **all of its school brands** (DL-22). The design system (`design-tokens/`, `prompts/`) defines how screens look; these files define what the screens are about. The AI design workflow reads them before it shapes a flow or generates a screen.

**Tóm tắt (VI):** Product truth mô tả Campus OS như những gì đã biết, mỗi phát biểu có nguồn (Known), giả định (Assumed, DL-xx) hoặc câu hỏi mở (Open, OQ-xx). Bằng chứng nằm ở `../research/`; product truth không tự nghĩ ra giải pháp cho các khoảng trống.

## Rules

1. **State what is known, and cite it.** Each statement is Known (with a source or research ID), Assumed (with a DL ID) or Open (with an OQ ID). See [CONVENTIONS.md](./CONVENTIONS.md).
2. **Gaps become open questions, not solutions.** Anything unknown goes to [`08-open-questions/`](./08-open-questions/open-questions.md).
3. **Research is evidence, not truth.** [`../research/`](../research/README.md) never overrides these files. A decision in [`07-decisions/`](./07-decisions/decision-log.md) is how product truth changes.
4. **Scope is every GSG brand.** GIIS is the brand described in most detail; label its facts as GIIS examples. SmartLearn is out of scope.

## Structure

Folders are numbered in reading order: context first, then users, domain, the two cross-cutting models (access, data governance), then design slices, decisions and open questions.

```
product-truth/
├── README.md                  ← you are here
├── CONVENTIONS.md             statuses, IDs, citations, where things go
├── index.json                 machine-readable map for the AI design workflow
├── 00-sources/                canonical source documents (Proposed)
│   ├── Global_Campus_OS_Product_Architecture_v1.md
│   └── Global_Campus_OS_Employee_Data_Dictionary_v1.md (+ .xlsx)
├── 01-context/                why the product exists and for whom
│   ├── product-brief.md       platform scope, bets, problem, coverage
│   ├── operating-context.md   GSG, 12 brands, countries, entities, systems
│   └── glossary.md            terms to use in screens
├── 02-users/
│   ├── stakeholders.md        user segments and decision-makers across GSG
│   └── roles-personas.md      Admin Platform personas P1–P5 mapped to GA/TM
├── 03-domain/
│   ├── modules-and-services.md   surfaces, 12 modules, 8 core services
│   ├── tenancy-and-scope.md      Group › Country › Campus hierarchy and its gaps
│   └── variation-catalogue.md    MC-01…MC-15: what varies, and coverage
├── 04-access/                 RBAC models
│   ├── README.md              access paths by actor, constraints, gaps
│   ├── role-catalogue.md      school roles, access roles, undefined families
│   └── rbac-model.md          six-layer model (Assumed, DL-16…DL-21)
├── 05-data-governance/        data controller model
│   ├── data-controller-model.md  legal roles, parties, known relationships
│   ├── data-classification.md    Tier 1/2/3
│   └── regulatory-context.md     Singapore, India; other countries open
├── 06-experience/             design slices
│   ├── README.md              slice structure and index
│   └── admin-platform/        brief, scenarios, screen map, flows/, rules-to-ui
├── 07-decisions/decision-log.md   DL-01…DL-22
├── 08-open-questions/open-questions.md   OQ-01…OQ-34
├── _templates/design-slice/   skeleton for a new slice
└── _archive/                  superseded reviews (gap analysis, 18 Oct)
```

## Index

| Folder | File | Answers | Status |
|---|---|---|---|
| 00-sources | [Product Architecture v1](./00-sources/Global_Campus_OS_Product_Architecture_v1.md) | Apps, 12 modules, shared core, roles, workflows, tenancy, data model, tech stack, integrations, security, roadmap (written for GIIS; scope note added) | Proposed |
| 00-sources | [Employee Data Dictionary v1](./00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) ([xlsx](./00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.xlsx)) | Employee master data, GA/TM access management | Proposed |
| 01-context | [product-brief.md](./01-context/product-brief.md) | Who runs it, for whom, bets, problem, how much product truth covers | Known / Proposed |
| 01-context | [operating-context.md](./01-context/operating-context.md) | GSG structure, brands, countries, entities, systems in use | Known (labelled) |
| 01-context | [glossary.md](./01-context/glossary.md) | Terms for organisation, access and data protection | Living |
| 02-users | [stakeholders.md](./02-users/stakeholders.md) | Seven user segments, decision-makers, observations | Known (desk research) |
| 02-users | [roles-personas.md](./02-users/roles-personas.md) | Admin Platform personas P1–P5, access role catalog A06, capability matrix | Assumed (DL-02) |
| 03-domain | [modules-and-services.md](./03-domain/modules-and-services.md) | Surfaces, modules with codes, core services, market coverage | Proposed / Assumed (DL-04) |
| 03-domain | [tenancy-and-scope.md](./03-domain/tenancy-and-scope.md) | Hierarchy levels, what each owns, dimensions it doesn't hold | Proposed / Open |
| 03-domain | [variation-catalogue.md](./03-domain/variation-catalogue.md) | MC-01…MC-15 mapped to product truth | Known |
| 04-access | [README.md](./04-access/README.md) | Access paths by actor, constraints, research gaps | Mixed |
| 04-access | [role-catalogue.md](./04-access/role-catalogue.md) | All role names and the families with none | Known / Assumed / Open |
| 04-access | [rbac-model.md](./04-access/rbac-model.md) | Six layers, evaluation algorithm, v1 scope, screen impact | Assumed |
| 05-data-governance | [data-controller-model.md](./05-data-governance/data-controller-model.md) | Legal roles, parties, relationships, flows needing an answer | Known / Open |
| 05-data-governance | [data-classification.md](./05-data-governance/data-classification.md) | Sensitivity tiers and design rules | Known / Assumed |
| 05-data-governance | [regulatory-context.md](./05-data-governance/regulatory-context.md) | SG and IN rules, timeline, regulators | Known (secondary legal) |
| 06-experience | [README.md](./06-experience/README.md) | Slice structure, slice index, how a screen is designed | Living |
| 06-experience | [admin-platform/](./06-experience/admin-platform/README.md) | v1 design slice: brief, scenarios, screen map (SCR-…), flows S1–S5, rules to UI | Proposed / Assumed (DL-01) |
| 07-decisions | [decision-log.md](./07-decisions/decision-log.md) | Every decision, its status and evidence | Living |
| 08-open-questions | [open-questions.md](./08-open-questions/open-questions.md) | Everything not yet known | Living |

## How the AI design workflow uses this folder

| Task | Read, in order |
|---|---|
| **Generate a screen** | `prompts/system.md` → the slice in `06-experience/<slice>/` (screen map → flow → rules to UI → sample data) → `01-context/glossary.md` → `03-domain/variation-catalogue.md` → OQ IDs the screen touches |
| **Screen touches access or permissions** | add `04-access/README.md`, `04-access/rbac-model.md` |
| **Screen touches several brands, countries, consent or sensitive data** | add `03-domain/tenancy-and-scope.md`, `05-data-governance/`, and the research file |
| **Shape product truth** (brief, persona, scenario, decision) | the research file first, then `01-context/` → `05-data-governance/`, then add DL or OQ entries |
| **Start a new design slice** | `06-experience/README.md` → copy `_templates/design-slice/` |
| **Review a design** | `08-open-questions/` (is anything open being shown as settled?), `07-decisions/` (Assumed vs Confirmed), research §10 |

The same routes are in [`index.json`](./index.json) for tools and prompts that load files programmatically.

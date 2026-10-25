# Template: design slice

Copy this folder to `product-truth/06-experience/<slice-name>/` and create the five parts below. Delete this paragraph and the guidance in italics. Follow [`../../CONVENTIONS.md`](../../CONVENTIONS.md): every statement is Known (with a source), Assumed (with a DL ID) or Open (with an OQ ID).

---

# <Slice name> — design slice brief

**Status:** Proposed, <date> · **Owner:** <name>
**Cites:** <research IDs, Architecture sections, DL IDs>
**Tóm tắt (VI):** <one or two sentences>

## Problem
*What is known about the problem, with evidence IDs. No solutions.*

## Users
*Which segments from `02-users/stakeholders.md` and roles from `04-access/role-catalogue.md`. Personas only where evidence exists; otherwise cite OQ-34.*

## Scope of this slice
| In scope | Notes |
|---|---|

## Non-goals

## Success metrics
| Metric | Target | How measured |
|---|---|---|

## Principles for designers

## Canonical sample data
*Reuse the set in `admin-platform/README.md` unless the slice needs more. Note which brands it covers (OQ-33).*

## Depends on
*Decisions (DL) this slice assumes, and open questions (OQ) that block parts of it.*

---

## Other files in the slice

| File | Required content |
|---|---|
| `scenarios.md` | One section per scenario: trigger, persona or role, outcome, screens, rules |
| `ia-screen-map.md` | Shell; navigation tree; screen table with ID, screen, roles, entities, MinV shell and blocks, key content, scenarios, state |
| `flows/README.md` + one file per scenario | User flow, state machine of the main object, screen states (empty, loading, error, no access, success) |
| `rules-to-ui.md` | Rule ID → UI behaviour; status vocabulary and colours; field visibility |

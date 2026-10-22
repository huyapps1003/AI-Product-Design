---
name: principles
description: Design and workflow principles every GCOS screen must follow, each with a concrete check an agent or dreaming pass can test against a screen or transcript.
layer: org
write: human-only
sources:
  - product-truth/00-product-brief.md (Principles for designers)
  - product-truth/05-rules-to-ui.md
  - product-truth/06-decision-log.md (DL-03, DL-07, DL-09, DL-10, DL-11)
  - prompts/system.md
reviewed: 2026-10-21
---

# Principles

IDs: `GP-n` product principles, `WF-n` workflow principles. Not to be confused with `P-…` UI patterns, `P0–P2` severity or persona numbers (see glossary).

**Tóm tắt (VI):** Mỗi nguyên tắc có cột **Check**: một câu hỏi có/không mà agent tự kiểm hoặc dreaming dùng để phát hiện vi phạm lặp lại. Nguyên tắc không có check thì không kiểm được, nên không đưa vào đây.

## Product principles

| # | Principle | Why | Check |
|---|---|---|---|
| GP-1 | **Show the boundary.** Any Team Management action shows the GA ceiling it sits inside (BR-07). | Module Admins otherwise assume they can grant anything. | Does every TM add/edit dialog show which modules × campuses the GA grant allows? |
| GP-2 | **Explain every permission.** Every allow or deny shows its source: grant, role, template version, scope, override. Pattern P-DISABLED for anything not allowed. | "Who can see what, and why?" is the product's core question. | Can a viewer trace each ALLOWED/DENIED row to a named source on the same screen? |
| GP-3 | **Never grant by side effect.** Transfers, rehires and campus changes open a review case; they never create access (BR-06, Decision D07). | Silent grants are the drift the product exists to stop. | After a transfer or rehire flow, is the end state a Case, not an active grant? |
| GP-4 | **Fail closed, visibly.** Missing scope or expired grant shows an explicit "no access" state, never an empty table (BR-17, DL-07). Pattern P-NOACCESS. | Empty tables read as "no data" and hide permission bugs. | Does every list have a distinct no-access state separate from its empty state? |
| GP-5 | **History is never overwritten.** Ending something is a dated action, not a delete (BR-03). Pattern P-HISTORY. | Audits and rehires depend on history. | Are there any Delete actions on employment, assignment or grant records? (Should be End / Revoke with a date.) |
| GP-6 | **Restricted looks restricted.** Confidential and Restricted fields carry the lock treatment and are masked by default for roles without the right (DL-11). Pattern P-LOCK. | Sensitivity must be visible, not implied. | Does each field match the visibility matrix in `05-rules-to-ui.md` for the viewing persona? |
| GP-7 | **Campus is always in view.** Current scope (Country › Campus) is visible in the shell on every staff screen. | Multi-campus users act on the wrong campus otherwise. | Is the scope switcher present and showing the current campus? |
| GP-8 | **Separation of duties.** HR maintains people, Global Admin sets the ceiling, Module Admin runs the team. No persona does another's job (DL-09). | Concentrated power defeats the audit story. | Does any screen let HR grant access, or a Module Admin edit employee data? |

## Workflow principles (how AI-assisted design is done here)

| # | Principle | Check |
|---|---|---|
| WF-1 | **Product truth wins.** When `product-truth/` and older mocks in `screens/` disagree, follow product truth. | Does the screen use names and values from product truth, not from older mocks? |
| WF-2 | **Trace every screen.** Each screen has an `SCR-…` ID from `03-ia-module-map.md`, a scenario from `02`/`04-flows/`, and the rules from `05` it must show. | Can the screen's ID, scenario and rule list be named? |
| WF-3 | **Canonical data only.** Use the sample set in `00-product-brief.md` (DL-03). | Any campus, person, domain or IdP not in that set? |
| WF-4 | **One status vocabulary.** Status chips use the label and colour from `05-rules-to-ui.md#status-vocabulary` (DL-10), and never colour alone. | Does every chip match the table and carry a text label? |
| WF-5 | **Tokens only.** Colours, spacing, type and radius come from `prompts/system.md` and `design-tokens/`. | Any hex value, spacing or font size outside the token set? |
| WF-6 | **Design every state.** Each screen covers the states its flow file lists (empty, loading, error, denied, and domain states). | Does the output include every state in the flow's screen-state table? |
| WF-7 | **Decide in the log, not in the screen.** A new assumption becomes a DL entry before it shapes a screen. | Did the session introduce a rule or behaviour with no DL or BR reference? |

## Precedence

Product truth › these principles › module memory › scratch. If two principles conflict on a screen, GP-2 and GP-4 win: correct, explained, fail-closed access beats convenience.

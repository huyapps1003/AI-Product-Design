---
name: memory-store
description: Contract for the design agent's memory store — layers, who may write where, how changes are versioned, how agents read it.
layer: meta
write: human-only
reviewed: 2026-10-21
---

# Memory store — Global Campus OS design agent

**Tóm tắt (VI):** `product-truth/` trả lời *sản phẩm là gì*. `memory/` trả lời *agent cần nhớ gì để thiết kế đúng*: tóm tắt ngắn, chỉ đường tới nguồn chuẩn, và những lỗi đã từng mắc. Memory **không chép lại** product truth; nó trỏ tới đó.

## One source of truth

| Folder | Holds | Canonical? |
|---|---|---|
| `product-truth/` | Product, data, rules, decisions | **Yes** |
| `design-tokens/`, `prompts/` | How screens look | **Yes** |
| `memory/` | Agent-facing digests, pointers, learned corrections | No — derived. When it disagrees with product truth, product truth wins and the memory line is fixed |

## Layers and write permissions

| Layer | Path | Changes how often | Who writes |
|---|---|---|---|
| Org | `memory/org/` | Rarely | Henry only. Dreaming may **propose** a diff |
| Domain | `memory/domain/` | Per design slice | Henry; dreaming proposes |
| Modules | `memory/modules/` | Per screen batch | Design agents may write; dreaming consolidates |
| Scratch | `memory/scratch/` | Per session | Any agent, freely. Never read as truth; dreaming promotes or deletes |

An agent never writes to a layer above its own. A lower layer may narrow an org rule, never contradict it.

## Reading (progressive disclosure)

1. Read every file's frontmatter `description` first.
2. Open the full file only if the task needs it.
3. Follow its `sources:` into `product-truth/` only for the exact section the task touches.

## Changing memory (versioning)

- Memory lives in Git. One commit per change, with the reason and the evidence (session, screen ID, or product-truth file) in the message.
- Changing an org line that reflects a decision: add or supersede the DL entry in `product-truth/06-decision-log.md` first, then update memory. Never edit a decision silently in memory.
- Rollback = `git revert`.

## Concurrency

Before writing, an agent records the file's commit hash it read. If the hash changed by write time, it re-reads and merges instead of overwriting.

## Frontmatter

```yaml
name:        # file stem
description: # one line, what's in it and when to open it
layer:       # org | domain | modules | scratch | meta
write:       # human-only | agent | any
sources:     # product-truth files this digest is derived from
reviewed:    # YYYY-MM-DD, last time a human confirmed it matches sources
```

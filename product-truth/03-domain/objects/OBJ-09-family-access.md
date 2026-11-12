---
id: OBJ-09
name: Family access
entities: [F01, R03]
owner: identity_access     # created by the system from a VERIFIED relationship (DL-25)
home: cell                 # data location of the child's enrolment (DL-55)
effective_dated: true
states: pending            # F01 status values not defined yet (AP-13)
capability_set: R03        # read attendance, grades, communication, own cases; edit profile (DL-28)
actions: []                # no manual grant; ends when the relationship or enrolment ends
events: [FamilyLinkCreated, FamilyLinkEnded]
variation: [MC-09, MC-10]
open: [OQ-10 consent, OQ-11 custody]
sample: pending   # student sample set (Lina Goh, SG-PGL) is in project files, not yet in the repo
patterns: [AP-03, AP-07, AP-13]
---

# OBJ-09 · Family access

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

An alternative access path, not a layer (DL-37). A parent who is also staff uses the context switch (AR-05).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

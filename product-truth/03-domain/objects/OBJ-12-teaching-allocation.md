---
id: OBJ-12
name: Teaching allocation
entities: [TE04, TE05, TE06, TE07]   # teaching group, group membership, teaching assignment, academic responsibility; replaces Employee DD D01, D02
owner: teaching            # Teaching domain (DL-43); module teaching_learning, prefix teaching (DL-131)
home: cell                 # campus data location (DL-47, DL-142); group views see counts and loads only
tenant_path: group/brand/brand-country/campus
operating_entity: none
effective_dated: true      # memberships, assignments and responsibilities have valid_from / valid_to (DL-24)
states:                    # TE06 assignment_status, DL-27 rule; colours proposed for review
  PROPOSED: default
  CONFIRMED: success
  ENDED: default
# Teaching group group_status: PLANNED default, ACTIVE success, CLOSED default
# Group kinds: CLASS (7B Mathematics), CHOICE (Grade 7 Hindi (L2) from 7A–7C), SPLIT (OQ-152)
field_groups:
  - name: Teaching groups and members
    tier: T1
    capability: teaching.allocation.view       # teachers: own groups; class teacher: own class
    edit: [teaching.allocation.manage]          # coordinator within band (DL-133)
  - name: Teaching assignments and load
    tier: T1
    capability: teaching.allocation.view
    edit: [teaching.allocation.manage]          # one LEAD per group (DL-134); load = sum of periods (DL-136)
  - name: Academic responsibilities
    tier: T1
    capability: teaching.allocation.view
    edit: [teaching.responsibility.manage]      # Principal (DL-135)
actions:
  - teaching.allocation.manage
  - teaching.responsibility.manage
  - teaching.load.view
events: [TeachingGroupCreated, GroupMembershipChanged, TeachingAssignmentChanged, ResponsibilityChanged, TeacherLoadOverMaximum]
# A CONFIRMED assignment sets the teacher's data scope to the group's students (DL-134, DL-73 pattern); never capabilities, never Tier 3.
# Responsibilities route approvals and suggest scope only (DL-16, DL-135).
variation: [MC-07]
open: [OQ-77, OQ-129, OQ-149, OQ-150, OQ-152, OQ-153, OQ-158, OQ-159]
sample: product-truth/03-domain/samples/teaching-timetable-sample.json
patterns: [AP-04, AP-05, AP-14]
---

# OBJ-12 · Teaching allocation

**Status:** Rewritten with the teaching & timetable dictionary v0.1, 24 Nov 2026 (DL-131 Confirmed; DL-132…DL-136 Assumed) · **Owner:** Henry
**Cites:** [teaching & timetable dictionary](../teaching-timetable-data-dictionary.md) · Employee DD D01, D02 · Arch § Users and roles · HB p.52, p.141–142 · DL-16, DL-73, DL-93, DL-116, DL-127

Who teaches which students which subject. The teaching group is what gets taught: a whole class for subjects everyone takes ("7B Mathematics"), a group across classes for chosen subjects ("Grade 7 Hindi (L2)" from 7A–7C) (DL-132). Members follow class placement and subject choice (DL-133). Each group has one lead teacher; the assignment gives that teacher data scope over the group's students, which is what Assessment (DL-93) and messaging (DL-127) read (DL-134). Coordinators, heads of department and academic supervisors are academic responsibilities that route approvals; they never grant access by themselves (DL-135, DL-16). Load limits are campus settings: show the value or "Not set".

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

---
id: OBJ-27
name: Timetable and lesson
entities: [TE01, TE02, TE03, TE08, TE09, TE10, TE11, TE12, TE13]   # bell schedule, bell period, room, period plan, timetable, timetable lesson, dated lesson, teacher absence, cover arrangement; replaces Employee DD D03
owner: teaching            # Teaching domain (DL-43); prefix teaching (DL-131); rooms until Campus Services (DL-141)
home: cell                 # campus data location (DL-47, DL-142)
tenant_path: group/brand/brand-country/campus
operating_entity: none
effective_dated: true      # bell schedules per year (DL-137); timetables are versions effective from a school day (DL-138)
states:                    # TE09 timetable_status, DL-27 rule; colours proposed for review
  DRAFT: default
  PUBLISHED: success       # screen label "Live"
  SUPERSEDED: default      # "Replaced"
  ARCHIVED: default
# Dated lesson lesson_status: SCHEDULED (no chip), CHANGED info, NEEDS_COVER error, COVERED secondary, CANCELLED warning
# Cover cover_status: NEEDED error ("Urgent" under an hour), ARRANGED success, DONE default, CANCELLED warning
# Clash severity: HARD error ("Must fix"), SOFT warning ("Check")
field_groups:
  - name: Bell schedules and periods
    tier: T1
    capability: teaching.timetable.view
    edit: [teaching.bell.manage]               # coordinator; Principal approves (OQ-150)
  - name: Rooms
    tier: T1
    capability: teaching.timetable.view
    edit: [teaching.room.manage]               # campus office (DL-141)
  - name: Period plans
    tier: T1
    capability: teaching.timetable.view
    edit: [teaching.plan.manage]
  - name: Timetable draft and clashes
    tier: T1
    capability: teaching.timetable.build
    edit: [teaching.timetable.build]
    approve: [teaching.timetable.publish]      # Principal (DL-138, OQ-150)
  - name: Published timetable and dated lessons
    tier: T1
    capability: teaching.timetable.view        # teachers whole campus; parents and students own (DL-142)
    edit: [teaching.lesson.change]
  - name: Teacher absences and cover
    tier: T1                                   # never the reason for absence (Staff and HR, T2; DL-143)
    capability: teaching.cover.view
    edit: [teaching.absence.record, teaching.cover.arrange]   # arranger OQ-162
actions:
  - teaching.bell.manage
  - teaching.room.manage
  - teaching.plan.manage
  - teaching.timetable.build
  - teaching.timetable.publish
  - teaching.lesson.change
  - teaching.absence.record
  - teaching.cover.arrange
events: [BellScheduleActivated, TimetablePublished, DatedLessonChanged, TeacherAbsenceRecorded, CoverArranged, CoverNeededUrgent]
# Hard clashes block publish (DL-139). A cover gives one-day scope only (DL-145).
variation: [MC-06, MC-07]
open: [OQ-110, OQ-147, OQ-148, OQ-150, OQ-151, OQ-153, OQ-154, OQ-155, OQ-156, OQ-157, OQ-160, OQ-161, OQ-162, OQ-163, OQ-164, OQ-165]
sample: product-truth/03-domain/samples/teaching-timetable-sample.json
patterns: [AP-01, AP-04, AP-05, AP-13, AP-14]
---

# OBJ-27 · Timetable and lesson

**Status:** Proposed with the teaching & timetable dictionary v0.1, 24 Nov 2026 (DL-131…DL-142 Confirmed; DL-143…DL-145 Assumed) · **Owner:** Henry
**Cites:** [teaching & timetable dictionary](../teaching-timetable-data-dictionary.md) · Employee DD D03 · Arch § Data model (Teaching, Organisation "Room") · HB p.9–14, p.22, p.34–35 · DL-96, DL-98, DL-99

When each group is taught, by whom, where. A bell schedule shapes the school day per campus and segment (DL-137). A timetable is a versioned weekly pattern built with live clash checks and published by the Principal (DL-138, DL-139). Each school day gets dated lessons from it; one-off changes happen on the dated lesson, never on the timetable (DL-140). When a teacher is away, their lessons need cover: the arranger picks a cover teacher from fair suggestions, or supervision, merge or cancel; the cover teacher sees the group's students for that day only (DL-143…DL-145). Period times, cycle, load and cover limits are campus settings: show the value or "Not set" (OQ-147, OQ-148, OQ-149, OQ-164).

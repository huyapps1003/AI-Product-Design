# Sample record sets

**Status:** Synthetic, for design only · **Owner:** Henry
**Cites:** [gsg-landscape § Sample scope nodes](../../01-context/gsg-landscape.md#sample-scope-nodes-for-screen-prompts) · OQ-36 · OQ-55
**Tóm tắt (VI):** Bộ dữ liệu mẫu (giả lập) cho các màn hình. Mỗi object card trỏ tới bộ mẫu của nó qua khóa `sample`.

Every person, number and address here is invented. Object cards point to their set through `sample:`.

| File | Objects | Viewer | Scope nodes |
|---|---|---|---|
| [`student-sample.json`](./student-sample.json) | OBJ-02 Student, OBJ-03 Enrolment, OBJ-04 Guardian | Lina Goh, Admissions, SG-PGL (other staff in `staff_users`) | GIIS SG-PGL, SG-ECS, IN-NOI (14 students); OWIS SG-OWD, Harrods KH-HLP and KH-HUP, Witty IN-WKB, Heath House UK-HTH, Regent MY-RKL (one student each) |

Records that test an open question carry a `_note` naming it: Sokha Chan (stage campus move, OQ-50), Lucas Lim (neighbouring brand, OQ-52), Aisyah Rahman (adult learner, OQ-54). Campus codes and Student IDs are placeholders (OQ-55, OQ-36). `SG-ECS` is not on GSG's public list and stays until GSG confirms it.

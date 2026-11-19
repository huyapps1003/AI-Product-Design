# Sample record sets

**Status:** Synthetic, for design only · **Owner:** Henry
**Cites:** [gsg-landscape § Sample scope nodes](../../01-context/gsg-landscape.md#sample-scope-nodes-for-screen-prompts) · OQ-36 · OQ-55
**Tóm tắt (VI):** Bộ dữ liệu mẫu (giả lập) cho các màn hình. Mỗi object card trỏ tới bộ mẫu của nó qua khóa `sample`.

Every person, number and address here is invented. Object cards point to their set through `sample:`.

| File | Objects | Viewer | Scope nodes |
|---|---|---|---|
| [`student-sample.json`](./student-sample.json) | OBJ-02 Student, OBJ-03 Enrolment, OBJ-04 Guardian | Lina Goh, Admissions, SG-PGL (other staff in `staff_users`) | GIIS SG-PGL, SG-ECS, IN-NOI (14 students); OWIS SG-OWD, Harrods KH-HLP and KH-HUP, Witty IN-WKB, Heath House UK-HTH, Regent MY-RKL (one student each) |

| [`admissions-sample.json`](./admissions-sample.json) | OBJ-16 Application (with OBJ-03) | Lina Goh, Admissions Officer, SG-PGL | 4 enquiries, 10 applications across every stage at SG-PGL, one IN-NOI and one OWIS SG-OWD; reuses the Sharma family (sibling) |
| [`classes-sample.json`](./classes-sample.json) | OBJ-15 Class (with OBJ-03) | Dr Anita Rao, Principal, SG-PGL (also Meera Joshi coordinator, Farah Ismail class teacher 7B, Lina Goh, Claire Tan OWIS Principal) | SG-PGL AY 2026-27 classes, plans (incl. AY 2027-28), 7B roster, needs-a-class queue, placement history; OWIS SG-OWD calendar-year rollover in progress; Harrods KH-HLP → KH-HUP stage promotion |
| [`organisation-sample.json`](./organisation-sample.json) | OBJ-13 Scope node and campus, OBJ-14 Legal entity | Daniel Ong, P2 Global Admin, whole group; Dr Anita Rao, P3 Campus Leader, SG-PGL | 12 brands, 21 brand-countries, 10 data locations; the nine campuses above with Noida segments; legal entities and operator links all pending (OQ-02) |
| [`fees-billing-sample.json`](./fees-billing-sample.json) | OBJ-23 Billing account and invoice (with OBJ-03, OBJ-04) | Siti Rahman, Finance Executive, GIIS Singapore (also Wendy Koh Finance Controller, Vivek Menon Country Director, Dr Anita Rao, Lina Goh, parents Neha and Vikram Sharma, Ankit Bansal IN-NOI) | GIIS SG fee plans and Term 2 billing runs (SG-PGL issued, SG-ECS in review), 12 billing accounts incl. Sharma siblings across two campuses and an employer sponsor, invoices, payments, credit notes, three refunds (Hana Kim paid; Chloe Tan and Meher Kapoor blocked on Not set rules), long-leave pause, FPS; IN-NOI and OWIS settings Not set. All amounts synthetic |
| [`assessment-sample.json`](./assessment-sample.json) | OBJ-19 Mark and report card (with OBJ-15, OBJ-03) | Meera Joshi, Academic Coordinator CBSE Grades 6-8, SG-PGL (also Dr Anita Rao, Dr Suresh Pillai Academic Supervisor, Arvind Menon and Farah Ismail teachers, parent Neha Sharma, Claire Tan OWIS, Rahul Bose IB DP SG-ECS) | GIIS SG CBSE 6-8 scheme v2 (approved) and v3 (submitted), templates, grade scales, Grade 7 Term 2 calendar from the Exam Schedule, mark sheets (8B Maths PT2 open), 7B Term 1 results, releases, Aarav's report card, four appeals, 7B promotion watch, OWIS Grade 4 promotion decisions feeding the Classes rollover, IB Year 2 predicted grades |

Records that test an open question carry a `_note` naming it: Sokha Chan (stage campus promotion, DL-54, DL-62), Lucas Lim (neighbouring brand, OQ-52), Aisyah Rahman (adult learner, OQ-54). Campus codes and Student IDs are placeholders (OQ-55, OQ-36). `SG-ECS` is not on GSG's public list and stays until GSG confirms it.

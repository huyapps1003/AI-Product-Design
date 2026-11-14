# Campus OS — Admissions: design brief for Claude Design

**Built from:** `product-truth/03-domain/admissions-data-dictionary.md` v0.1 (PR #14), OBJ-16 card, Student Directory Spec v1.0, Architecture v1 § Admission to enrolment, GIIS Student Handbook 2025-26 (HB)
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands. GIIS is the detailed example; one OWIS and one Noida application show other contexts.
- Scope: a new family's path **from first enquiry to first day**. Withdrawals, refunds, transfers, billing and promotion are other modules.
- Viewer: **Lina Goh, Admissions Officer, SMART Campus Punggol (SG-PGL)**. Today is **12 Nov 2026**. Other staff: Marcus Lee (Admissions Manager, GIIS Singapore), Dr Anita Rao (Principal), Deepa Krishnan (Counsellor, SEN), Priya Nair (assessor).
- Staff web app, desktop. Parent screens are a later, separate view.

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **Enquiry** | First contact from a family; may name several children (DL-56) | Lead (in UI) |
| **Application** | One child, one campus + programme + grade + academic year (DL-56) | Admission form |
| **Offer** | A seat offered to an application; creates the Student ID and an Offered enrolment (DL-57) | Admission letter |
| **Contract** | Signed agreement with the family; cooling-off starts at full signature | |
| **Student Pass case / MOE approval** | Government approval needed before the child can start | Visa |
| **Enrolment** | The student placed in a campus, grade and class (Student Directory) | Admission |
| **Class**, **Segment**, **Student ID** | As in the Student Directory brief (DL-29, DL-67) | Section, admission number |
| **AEPP** | English placement *inside* a regular enrolment (DL-65) | AEPP programme / class enrolment |

## 3. Pipeline and chips

Enquiry chips: New · info, Contacted · info, Visit booked · info, Visited · info, Application started · success, Closed (lost) · default, Duplicate · default.

Application chips (OBJ-16): Draft · default, Submitted · info, Documents pending · warning, Assessment · info, Under review · info, Waitlisted · warning, Offered · info, Accepted · success, Enrolled · success, Declined · default, Rejected · error, Withdrawn · default.

Offer: Issued · info, Accepted · success, Declined · default, Expired · default, Rescinded · error.
Contract: Draft, Sent, Signed, In cooling-off · warning, Active · success, Cancelled in cooling-off, Terminated.
Fee: Due · warning, Paid · success, Waived · default, Refunded · default.
Approval case: Preparing, Submitted · info, Approved · success, Rejected · error.

How it maps to the student's enrolment (Student Directory chips): offer issued → **Offered**; offer accepted + contract signed + registration fee paid → **Confirmed**; approval approved (or not needed) + start date → **Enrolled**; declined/expired/rejected approval → **Cancelled**.

## 4. Rules that change the UI

1. **Siblings and returning families reuse their Person** (DL-58). Matching suggests "Possible existing family"; staff confirm, never auto-merge.
2. **No student record before the offer** (DL-57). Application screens show applicant details; the Student ID appears once the offer is issued.
3. **Rule values are campus settings, no group default** (DL-59). Show "7 working days (campus setting)". A campus without a value shows **Not set** and the step is blocked with "Ask your Admissions Manager to set this".
4. **SEN evaluations and passports are restricted** (DL-60). Application shows only the SEN outcome (Can support / Needs discussion / Cannot support) with a lock and a link for the counsellor. Passport and birth certificate files are masked with "Reveal (recorded)".
5. **Capacity** (DL-61). Each grade shows seats: limit, enrolled, held (open offers + confirmed), free. When full, "Issue offer" is disabled for officers; Admissions Manager can override within the campus buffer with a reason; beyond that the Principal approves. Waitlist order: siblings, then staff children, then completion date. A freed seat sends a time-limited offer to the first on the list.
6. **Cooling-off** (GIIS SG setting, HB p.233): 7 working days from full signature. Show a countdown on the contract and the application.
7. **Fee refund on rejection** (GIIS SG setting, HB p.242): application and registration fees are non-refundable, except if ICA rejects the Student Pass or MOE rejects, less an admin charge.
8. **Enrolment handoff** (SR-07): on Enrolled the student gets a class, timetable, bus route and logins; parent access comes from a verified guardian link, never from the application.
9. **No delete.** Close, withdraw, rescind or cancel with a reason.

## 5. Who sees what (summary)

Admissions officer manages everything at their campus. The Admissions Manager sees the country and overrides capacity. The Principal views and approves over-limit offers. Assessors see only the applications assigned to them. The counsellor manages SEN evaluations. Finance manages fees. Group leadership sees counts only.

## 6. Pending decisions (show "Pending decision")

OQ-62 can families apply online, and with what login · OQ-63 enquiry naming two brands (Sara Haddad) · OQ-64 which tests are required and whether parents see results · OQ-66 who decides and issues offers, offer validity · OQ-67 contracts and e-signature outside Singapore · OQ-68 fee amounts · OQ-69 retention and marketing consent · OQ-70 agents · OQ-71 returning students and deferred starts · OQ-72 document list per country.

## 7. Suggested screens

1. Enquiry queue · 2. Pipeline board · 3. Application detail (tabs: Overview, Documents, Assessments, Offer and contract, Fees, Approval case, History) · 4. Capacity and waitlist · 5. Issue offer (with over-limit override) · 6. Contract and cooling-off · 7. Student Pass case · 8. Enrolment handoff · 9. Campus admission settings with "Not set" states.

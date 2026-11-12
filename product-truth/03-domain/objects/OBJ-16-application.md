---
id: OBJ-16
name: Application
entities: [AD01, AD02, AD03, AD04, AD05, AD06, AD07, AD08, AD09, AD10]
owner: admissions
home: cell                 # data location of the campus applied to; unrouted enquiries OQ-63
tenant_path: group/brand/brand-country/campus
operating_entity: copied at creation   # DL-41; controller unnamed until OQ-06
effective_dated: false     # status history kept as events
states:                    # application_status, DL-27 rule; colours proposed for review
  DRAFT: default
  SUBMITTED: info
  DOCUMENTS_PENDING: warning
  ASSESSMENT: info
  UNDER_REVIEW: info
  WAITLISTED: warning
  OFFERED: info
  ACCEPTED: success
  ENROLLED: success
  DECLINED: default
  REJECTED: error
  WITHDRAWN: default
# Offer issued creates ST01 + ST02 OFFERED (DL-57); accepted + contract signed + fee paid = ST02 CONFIRMED (SR-AD-04)
field_groups:
  - name: Enquiry, applicant and contact details
    tier: T2
    capability: admissions.application.view
    edit: [admissions.application.edit]
  - name: Documents (passport, birth certificate)
    tier: T3
    capability: admissions.document.review   # plus record ACL
  - name: Assessment outcome
    tier: T2
    capability: admissions.application.view
    edit: [admissions.assessment.record]
  - name: SEN evaluation outcome
    tier: T3
    source: Wellbeing      # outcome code + link only (DL-60)
  - name: Offer, contract, admission fees
    tier: T2
    capability: admissions.application.view
    edit: [admissions.offer.issue, admissions.contract.issue]
  - name: Government approval case
    tier: T3
    source: OBJ-10
actions:
  - admissions.enquiry.create
  - admissions.enquiry.assign
  - admissions.application.decide     # OFFER, WAITLIST, REJECT; who decides OQ-66
  - admissions.offer.issue
  - admissions.offer.rescind
  - admissions.contract.issue
  - admissions.approval_case.manage
  - admissions.enrolment.create       # handoff (SR-AD-07)
events: [EnquiryReceived, ApplicationSubmitted, ApplicationDecided, OfferIssued, OfferResponded, ContractSigned, ApprovalCaseDecided, StudentEnrolled]
variation: [MC-01, MC-05, MC-06, MC-07, MC-12, MC-14]
open: [OQ-45, OQ-62, OQ-63, OQ-64, OQ-65, OQ-66, OQ-67, OQ-68, OQ-69, OQ-70, OQ-71, OQ-72]
sample: product-truth/03-domain/samples/admissions-sample.json
patterns: [AP-01, AP-04, AP-05, AP-13, AP-14]
---

# OBJ-16 · Application

**Status:** Proposed with the admissions dictionary v0.1, 12 Nov 2026 (DL-56…DL-61 Assumed) · **Owner:** Henry
**Cites:** [admissions dictionary](../admissions-data-dictionary.md) · DL-41, DL-42, DL-57, DL-60

One application per child per target (DL-56). Rule values from the GIIS Handbook (7 working days cooling-off, fee refund on ICA/MOE rejection) are settings; show them as the campus's setting, never as fixed text (DL-59).

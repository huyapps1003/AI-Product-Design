# Campus OS — Admin Platform: design brief for Claude Design

**Built from:** `product-truth/03-domain/admin-platform-data-dictionary.md` v0.1, OBJ-01, OBJ-07, OBJ-08, OBJ-11, OBJ-32…OBJ-36, organisation dictionary, RBAC decisions DL-32…DL-53, DL-69, S5 access review flow
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands (DL-22). v1 is the staff web app (DL-118); the Admin Platform is a set of sections inside it (DL-05), not a separate product.
- Scope (DL-165, Assumed): **how GSG and each brand set Campus OS up, who can sign in, how access is given, reviewed and explained, and the data protection settings every module reads.**
- Viewers: **Daniel Tan**, Global admin, GSG platform team (desktop, main viewer) · **Aisha Karim**, Brand admin for OWIS (new role, DL-178) · **Dr Anita Rao**, Campus leader, SMART Campus Punggol (approves support sessions and Tier 3 grants, attests access each term) · **Leo Fernandes**, GSG support engineer (no standing access to school data).
- Today is **Monday 30 Nov 2026, 09:30 SGT**. OWIS Digital Campus (SG-OWD) is moving onto Campus OS for 4 Jan 2027 (sample date).
- All admin screens are desktop first (DL-06). Dr Rao's approvals also work on a phone.

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **Setup run** | The go-live checklist for a campus or brand-country (DL-167) | Onboarding project, implementation |
| **Switch on** / **Live** / **Pilot** | Whether a module is used at a campus, and from when (DL-166) | Enable, license, activate |
| **Legacy systems** | Which system is the source of truth for each field group until its cutover date (DL-45, DL-168) | Authority register (in UI), integrations |
| **Data location** | Where a country's data lives, "Data location: Singapore" (DL-47) | Cell, region, data home |
| **Account** | A person's one Campus OS login (DL-42) | User ID, profile |
| **Sign-in** | How staff log in through the one GSG-wide provider, and each brand's move onto it (DL-169, OQ-210) | SSO config, IdP (in UI) |
| **Route** | How someone got access: From position template, From allocation, By request, Added by admin (DL-171) | Source type |
| **Support session** | Approved, read-only, time-boxed GSG support access to one campus (DL-175) | Impersonation, admin login |
| **Break-glass** | Security-only session without prior approval | Emergency access |
| **Tier 3 grant** | Standing access to one kind of restricted record (DL-69) | Sensitive permission |
| **Access review** | Termly keep / reduce / remove check by the Campus Leader (DL-174) | Audit, recertification |
| **Not set** | A setting with no value; the module behaves as undecided, never a hidden default | Default (unless one is defined) |

## 3. Chips

Account: Invited · info, Active · success, Suspended · warning, Disabled · error, Closed · default; flag "Dormant {n} days" · warning outlined.
Module switch-on: Planned {date} · info, Pilot · secondary, Live · success, Paused · warning, Off · default.
Setup run: {n} of {m} done · info, Ready for sign-off · success, Live · success. Task: Done · success, Blocked · error, Not needed · default.
Legacy systems: From {system} · info soft, Cutover {date} · info, Campus OS · success, Not listed · error.
Support session: Waiting for {approver} · info, Live · ends {time} · warning, Ended · default, Declined · error, Break-glass · error.
Template: Draft · default, v{n} published · success; "{n} on older" · warning.
Anything open: outlined warning "Pending · OQ-xx".

## 4. Rules that change the UI

1. **Access explains itself.** Every grant and membership shows its route, and "Why can X see Y?" lists the chain: module grant, switch-on, membership, template, scope, record (DL-38, DL-171).
2. **A grant is a ceiling, a membership gives work.** Nobody gets data from a grant alone (DL-02).
3. **No members where a module isn't Live or Pilot** (DL-166). A request for such a module waits and says why.
4. **Go-live is gated.** "Send for sign-off" is disabled until every blocking task is done; most tasks tick themselves from the owning record (DL-167).
5. **Before cutover, legacy fields are read-only** with a source chip; conflicts go to a named reviewer (DL-45).
6. **Accounts follow employment** but dormant accounts are only flagged, never suspended automatically (DL-170, OQ-204).
7. **Brand admins stay inside their brand** and can't edit roles or templates (DL-172, DL-178).
8. **A published template never changes.** Publishing a new version shows how many members are on older versions (DL-172).
9. **Support has no standing access.** A session is one campus, named modules, read-only, max 4 hours, Tier 3 masked, approved by the Campus Leader, every screen logged (DL-52, DL-175).
10. **Missed reviews never remove access.** Undecided rows escalate to the brand-country head (DL-174).
11. **Never name a controller or operating entity.** Use "[data controller — OQ-06]" and "[operating entity — OQ-02]" (AR-04). Retention with no period deletes nothing.
12. **Other countries show counts** unless the viewer has access there (AR-01).

## 5. Screens (prototype IDs, DL-179 proposed navigation)

Setup: SCR-ADM-01 Home · 02 Brands and campuses + setup run · 03 Modules · 04 Legacy systems · 05 Settings · 06 Sign-in. People and access: 07 Users · 08 User access detail · 09 Roles and templates · 10 Requests · 11 Special access · 12 Access review. Data protection: 13 Purposes, retention, notices. Audit: 14 Audit log.

## 6. Open (show as Pending)

OQ-02 operating entities · OQ-06 controller · OQ-09 lawful basis · OQ-10 consent · OQ-21 data locations beyond SG and IN · OQ-27, OQ-28 legacy systems and cutover dates · OQ-43, OQ-185, OQ-199 retention periods · OQ-192 position template contents · OQ-194 OWIS sign-in · OQ-196 go-live approver · OQ-197 go-live order · OQ-200 purpose list · OQ-201 data protection contacts · OQ-202 notice per entity · OQ-204 dormant threshold · OQ-205 approver when the Campus Leader is the grantee · OQ-206 reviews above campus · OQ-207 break-glass · OQ-209 parent sign-in.

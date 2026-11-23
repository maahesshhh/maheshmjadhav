# Gap register: Ohioans HHC Referral Bot (Careport), BRD v1.25

Input: `02-requirements.json`, revision r2 (112 requirements, 16 conflicts, 17 assumptions, 10 missing inputs), plus 5 master files dated 2026-11-23.

**r2 changes (2026-11-23):** the masters resolved GAP-01, and partly resolved GAP-03 and GAP-17. GAP-04 is deferred because the Payer Portal BRD is not available. Profiling the masters added GAP-39 to GAP-47.
**BRD-GAP** = the BRD is silent or ambiguous. **AI-RECOMMENDATION** = a suggestion from this review, not a requirement.
Every "Suggested default" is a suggestion only, and needs the business or engineering owner to confirm it.

## Operations reviewed

| Operation | Requirements | Coverage |
|---|---|---|
| Portal login (Careport, Kantime, payer portals) | REQ-001, 064, 104 | Partial (OTP, sessions and password reset not specified) |
| Referral discovery and monitoring | REQ-002, 004, 005 | Partial (no duplicate key; no limit on how long a masked referral is watched) |
| Screen and PDF extraction | REQ-011, 014-017 | Partial (no confidence or fallback rules) |
| Document combine, SharePoint, upload | REQ-018-020, 084 | Partial (no timeout or failure handling) |
| Kantime existing-patient lookup | REQ-021-029 | Partial (name+DOB match only; report data can be stale) |
| Decision rules (Medicare, age, keyword, payer, census, facility) | REQ-007-010, 030-063 | Mostly covered; boundaries and order of checks open |
| Payer portal eligibility | REQ-064-069 | Not covered here (separate Payer Portal BRD) |
| Payer identification by identifier | REQ-092-098 | Partial (tie-break depends on row order) |
| Kantime chart (new, readmit, non-admit) | REQ-058, 071-090 | Covered for the happy path; partial success not covered |
| Asks, Alerts, emails, CareFlo logging | REQ-012, 070, 101 | Partial (no Ask timeout, no catalogue, PHI handling not covered) |
| Exceptions, retries, audit | none | Not covered |

## Register

| ID | Class | Tag | Severity | Req | Finding | Question | Suggested default |
|---|---|---|---|---|---|---|---|
| GAP-01 | ~~Critical blocker~~ **Resolved r2** | BRD-GAP | — | REQ-007, 013 | The "Approval Required List" sets Intake Review Flags 1 and 2, but the BRD never says what it is or where it lives. | What is the Approval Required List, where is it kept, who maintains it, and what is the match key? | A tab in the Insurance and Service Areas Google sheet, matched on normalised name + DOB. |
| GAP-02 | Critical blocker | BRD-GAP | Critical | REQ-008, 023 | The Medicare bypass accepts the referral and creates a chart **without** the Kantime report lookup. An Active, PSOC or Transfer patient would get the "new patient" comment and possibly a duplicate chart. | Must the bypass first check the two cached Kantime reports? That check is a local file lookup and takes seconds. | Yes. Run the report lookup before the bypass; it does not slow the response. |
| GAP-03 | Critical blocker (partly resolved r2: 4 of 5 masters received; the Service Area sheet is still missing; see GAP-39 to GAP-42) | BRD-GAP | Critical | REQ-037, 048-063, 074-090 | Masters not supplied: Service Area, Insurance plan master, Referral Source, Restricted Facility. Column letters are inferred from the prose. | Send current copies, and say who owns each, where it is kept (Google Sheets or the CareFlo master) and how it is versioned. | None. Build is blocked. |
| GAP-04 | **Deferred r2** (Payer Portal BRD not available) | BRD-GAP | High | REQ-066, 085, 086 | The eligibility steps for the 8 payer portals are in a separate "Payer Portal BRD" that was not supplied. | Send the Payer Portal BRD. Is it in scope for this build? | Treat it as a separate work package. This build stops at the portal outcome (eligible, inactive, not found). |
| GAP-05 | Critical blocker | BRD-GAP | High | All | v1.24 says the requirements follow "the approved flow diagram from Jaimie", but section 4 only contains the text "New flow diagram". | Send the approved To-Be diagram. Does it override BRD prose where they differ? | BRD prose is the working authority until the diagram is received. |
| GAP-06 | Business clarification | BRD-GAP | High | REQ-041-045, 075 | Section 5.13 says paediatric is 0-18 and 5.24 says "<18". A patient aged exactly 18 is undefined. | Is an 18-year-old paediatric or adult? | Under 18 is paediatric; 18 and over is adult. |
| GAP-07 | Business clarification | BRD-GAP | High | REQ-003, 105 | 14+ referral portals are named, but screen steps exist only for Careport. | Is this build Careport-only? | Careport only; the other portals become later phases. |
| GAP-08 | Business clarification | BRD-GAP | High | REQ-023, 024 | Kantime lookup uses patient name + DOB only. Not covered: similar names, hyphenated or middle names, nicknames, two patients with the same name and DOB. | What is the exact match rule? Can SSN or MBI be used as a tie-break? What happens with more than one hit? | Normalised last+first name + exact DOB. If more than one hit, raise an Intake Review Ask. |
| GAP-09 | Business clarification | BRD-GAP | High | REQ-002, 005 | No duplicate-prevention key: the same referral seen again after a bot restart, the same patient re-referred, or a multi-branch referral seen once per site. | Which key identifies a referral already processed? | Careport Referral ID + site, stored in the CareFlo DB. |
| GAP-10 | Business clarification | BRD-GAP | High | REQ-012, 070, 095 | No timeout or escalation for Asks (Intake Review, Mandatory EMR, Basic Info Missing). If the team doesn't answer, the 1-hour SLA and the first-come-first-served opportunity are lost. | What is the Ask response SLA? Who gets escalated to? What happens to the referral while it waits? | Reminder at 20 minutes and escalation to the Intake lead at 40 minutes; the referral stays pending. |
| GAP-11 | Business clarification | BRD-GAP | Medium | REQ-004 | Masked referral: the bot "continues to monitor", with no limit. It is also unclear whether the "Decision Pending Review" response is sent again. | How long should the bot watch a masked referral? Should "Please unmask" be sent once or again? | Send once, then watch until unmasked or until the status moves off Not Placed. |
| GAP-12 | Business clarification | BRD-GAP | High | REQ-033, 041-069 | On the Medicare PDF path (5.10) the referral is accepted straight away. That means it skips the paediatric, keyword, dummy-payor, UHC/Humana, census, restricted-facility and payer-portal checks. | Confirm that traditional Medicare skips all of those checks, including census Red/Yellow and exception keywords such as Infusion. | As written, but confirm keywords and census, since these are the riskiest to skip. |
| GAP-13 | Business clarification | BRD-GAP | Medium | REQ-056 | Partial order acceptance (for example 1 of 2 orders can't be staffed): the referral is accepted, but what happens to the unaccepted discipline? | Should the unaccepted discipline be left out of Kantime or flagged? Should the referral source be told? | Accept, record the unaccepted disciplines in CareFlo and in the Kantime chart notes, and email the Intake team. |
| GAP-14 | Business clarification | BRD-GAP | Medium | REQ-068, 069 | "Inactive" vs "not eligible" is undefined. Future-effective coverage and coverage ending soon are not covered. | Define both. How should future-effective coverage and coverage ending within 30 days be handled? | Inactive or future-effective sets Flag8 for review; active but plan not accepted is a decline. |
| GAP-15 | Business clarification | BRD-GAP | Medium | REQ-080 | Only a Medicare secondary payer "in Availity" is handled. Non-Medicare secondaries and tertiary payers are not covered. | Should non-Medicare secondaries be added in Kantime? | Add the secondary only when it is Medicare; record others in CareFlo notes. |
| GAP-16 | Business clarification | BRD-GAP | Medium | REQ-093, 094 | "Two or more identifier matches: pick the first row." The result depends on the sheet's sort order, so re-sorting changes the result. | Is row order a deliberate priority? | Add an explicit Priority column to the Insurance master. |
| GAP-17 | Business clarification (now High, see GAP-43) | BRD-GAP | High | REQ-047 | Exception keyword matching is not specified: case, partial words, negation ("no infusion needed"). | Should matching be whole-word? Must negation be handled? | Case-insensitive whole-word match. Negation is a known limitation and still raises a flag. |
| GAP-18 | Business clarification | BRD-GAP | Medium | REQ-041, 044, 060 | Decline Response values differ between sections ("Decline" vs "No, unable to accept patient"). | Send the actual Careport Response and Response Reason dropdown values. | Use each section as written and confirm in Careport production. |
| GAP-19 | Business clarification | BRD-GAP | Medium | REQ-063 | The Mercy "partial match (extra letters/numerics handled)" rule is undefined. | Give 3-5 real examples of locations that should match and should not. | Normalised "contains" match after removing spaces and punctuation. |
| GAP-20 | Business clarification | BRD-GAP | Medium | REQ-024-028 | Existing patients (Active, Transfer, PSOC) skip the service area and payer checks. A patient who moved or changed payer is only reported in the mismatch email. | Confirm that is intended. | As written. |
| GAP-21 | Business clarification | BRD-GAP | Medium | REQ-091 | Manual decline: the Intake team's CareFlo comment may not be one of the 8 mapped comments. An "EPIC portal" is mentioned but is not in any portal list. | How should an unmapped comment be handled? Is EPIC in scope? | Restrict the CareFlo comment to a dropdown of the 8 values. EPIC is out of scope. |
| GAP-22 | Business clarification | BRD-GAP | Medium | REQ-099 | "Interested": the chart is created after the Careport update, but the Kantime intake status and the follow-up are not given. | Which Kantime status applies, and what happens when the source answers? | Status "2- Coordination"; the follow-up is manual. |
| GAP-23 | Business clarification | BRD-GAP | Low | REQ-052, 057 | A No UHC/Humana decline uses the non-admit reason "Census at maximum capacity", although the reason given to the source is insurance in the area. | Is "Territory at Capacity" more correct? | As written. |
| GAP-24 | Business clarification | BRD-GAP | Low | REQ-076 | Camel-case names: Mc/Mac, O'Brien, hyphenated, and suffixes (Jr, III) are not covered. | What casing rules apply? | Title-case each part and keep the apostrophe and hyphen; keep suffixes uppercase. |
| GAP-25 | Technical clarification | BRD-GAP | High | REQ-001, 064, 104 | OTP by email and password reset are required for Kantime, Careport, Ability and UHC. Bot accounts and concurrent sessions are not defined. | Will there be dedicated bot accounts? Which mailbox receives the OTP and how does the bot read it? What is the password-rotation policy? How many concurrent sessions are allowed per portal? | Dedicated bot accounts, OTP read through Microsoft Graph or IMAP, rotation alert 7 days before expiry. |
| GAP-26 | Technical clarification | BRD-GAP | High | REQ-104 | There is no UAT for Careport or any payer portal, so testing happens in production on real patients. | Are test referrals or a sandbox available? Who approves production test runs? | A dry-run mode that does everything except submit, plus a supervised pilot. |
| GAP-27 | Technical clarification | BRD-GAP | High | REQ-102, 103 | Throughput: on the full path, combining documents takes minutes, plus extraction, the payer portal and the Kantime chart. Peak is 11am-4pm at 60-90 a day, against a 1-hour SLA. | How many bot runners will there be? Can the referral response be separated from chart creation? | Two queues: decide and respond first; create the chart and upload after. |
| GAP-28 | Technical clarification | BRD-GAP | High | REQ-011, 015 | PDF extraction from scanned documents, and extraction of services and orders, have no confidence threshold or fallback. A wrong order affects the census and staffing decision. | What is the minimum confidence? What should happen when it's low? | Low confidence on payer, policy or orders raises a Mandatory EMR Ask or Intake Review Ask. |
| GAP-29 | Technical clarification | BRD-GAP | High | REQ-058, 067, 072-087 | Partial success is not covered: the referral is accepted in Careport, but the Kantime chart fails halfway. | Who is told, and how does the bot resume without creating a duplicate? | CareFlo status "Accepted - chart pending", an alert to Intake, and a resume via Check Duplicate. |
| GAP-30 | Technical clarification | BRD-GAP | Medium | REQ-018 | Combine documents: no timeout if the job stays "Pending" or fails. | What is the timeout? | Poll every 30 seconds for up to 10 minutes, then download each document separately. |
| GAP-31 | Technical clarification | BRD-GAP | Medium | REQ-038, 039 | County API: only an empty county value is covered, not API errors or timeouts. Google API key and billing, and whether Randymajors can be used, are open. | Which provider, which key, and what happens on errors? | 3 retries, then the zip-only check if non-Medicare, and log it. |
| GAP-32 | Technical clarification | BRD-GAP | Medium | All portal steps | Portal timeouts, session expiry, maintenance windows, retry and backoff, and escalation are not defined for any application. | What is the retry policy, and who is escalated to? | 3 retries with backoff, then flag for manual and send an alert to Intake. |
| GAP-33 | Technical clarification | BRD-GAP | High | REQ-014, 101 | PHI handling is not covered: SSN and DOB are captured; screenshots and logs contain PHI; combined PDFs are emailed to distribution lists; the SharePoint folder has no stated permissions or retention. | What are the HIPAA logging rules, retention and email encryption requirements? | Mask SSN in logs, no screenshots in production logs, encrypted email, 30-day retention on SharePoint. |
| GAP-34 | Technical clarification | BRD-GAP | Medium | REQ-070, 101 | The CareFlo database schema, status vocabulary and Ask/Alert catalogue (MIS-008) are not defined. | Send the CareFlo data contract and the Ask/Alert list. | Define them in the SDD. |
| GAP-35 | Known limitation | AI-RECOMMENDATION | Medium | REQ-021, 022 | A patient created in Kantime between report downloads (up to 30 minutes, or 5 hours for RM-Intake) is treated as new. Kantime Check Duplicate at chart time reduces the risk but doesn't fix the wrong Careport comment. | Accept this? | Accept, and rely on Check Duplicate. |
| GAP-36 | Known limitation | AI-RECOMMENDATION | Low | All UI steps | Careport and Kantime UI changes will break selectors. | None needed. | Central selector repository plus a daily login and smoke-test health check. |
| GAP-37 | Recommended enhancement | AI-RECOMMENDATION | Medium | REQ-007-063 | 25 versions in about 2 years, mostly rule changes. Hard-coded rules will be expensive to maintain. | None needed. | Keep the decision rules in a rule engine or config table, separate from the UI bots. |
| GAP-38 | Recommended enhancement | AI-RECOMMENDATION | Low | REQ-101 | Nothing checks that Careport decisions and Kantime charts stay in step. | None needed. | Daily reconciliation report from CareFlo. |
| GAP-39 | Critical blocker | BRD-GAP | Critical | REQ-008, 048-055, 077, 085, 092-096, 109 | **Payor Mapping key columns are empty.** In all 1,156 rows, Agency, Medicare, Commercial, EMR Source, Identifier and Contract are empty; EMR Payor Name is filled in only 2 rows. Without them, the Medicare bypass, Dummy-payor, UHC/Humana, agency-appropriate, census-colour, identifier and Kantime-payer rules can't run. | Is this export incomplete, or are these values kept somewhere else (for example in the old Google sheet)? Please re-export with the columns filled. | Blocks build and testing of the decision rules. |
| GAP-40 | Critical blocker | BRD-GAP | Critical | REQ-041-044 | **No Paediatric (Purple) plan flag** exists in Payor Mapping. | Which column or source marks a paediatric plan? | Add a Paediatric Y/N column to Payor Mapping. |
| GAP-41 | Critical blocker | BRD-GAP | High | REQ-081, 088-090 | **Facility Master routing columns are empty.** In all 928 rows, CC1, Sales Person, Admission DL, Marketer Email, Admission Source, Referring Physician and Agency Name are empty. There is no Kantime facility name column, and the dummy "Ohioans HomeHealth Care" is missing. | Please re-export with the routing columns filled and add the dummy facility. | Until then, team, marketer and email routing would all fall back to Team 2. |
| GAP-42 | Critical blocker | BRD-GAP | Critical | REQ-037, 052-056, 074, 078 | **The Service Area mapping sheet was not supplied.** It holds zip, county, location, limitation, staffing per order, payer colour per order and team. | Please send it. | Service area, No UHC, census, staffing, location and team rules are blocked. |
| GAP-43 | Business clarification | BRD-GAP | High | REQ-047 | The Exception Keywords Master has short tokens (ALS, AMA, MS, MAG, C9, IVF, PEG, DNR) and overlapping ones (STAGE I / II / III / IV). A substring match would flag "Ms. Smith", "alsо" and "magnetic", and "STAGE I" would match every stage. | Confirm whole-word matching. Should "MS" be removed or changed to "MULTIPLE SCLEROSIS"? | Whole-word match on normalised text, and rename the short tokens to full terms. |
| GAP-44 | Technical clarification | BRD-GAP | Medium | REQ-046, 111 | The Exception Keywords Master "Line of Business" column contains exported code (`const names = { 1: 'Hospice', 2: 'Home Health' } …`) instead of a value. | Please re-export with the actual LOB values. | Treat all 65 keywords as Home Health for now. |
| GAP-45 | Business clarification | BRD-GAP | High | REQ-060, 062, 063, 107 | The Restricted Facility master doesn't match the BRD's format: there is no Kantime name; Careport Location is filled for 1 of 35 rows; the ProMedica and Mercy location codes are missing; 26 of 31 names aren't in the Facility Master; names contain non-breaking spaces; "TENET - DMC SINAI-GRACE HOSPITA" is a truncated duplicate. | How should a Careport facility be matched to a restricted facility: exact name, contains, or a mapping? Please add the ProMedica and Mercy location rows. | Normalised exact-name match, with location codes added for ProMedica and Mercy. |
| GAP-46 | Business clarification | BRD-GAP | Medium | REQ-013 | Approval Required List: 3 rows have a future DOB (probably two-digit-year errors), 58 names contain spaces or punctuation, EMR MRN is empty in every row, and 1 name appears with two DOBs. | Please correct the future DOBs. Can MRN be filled to give a stronger match key? | Match on normalised name + DOB; report bad rows at load time (REQ-112). |
| GAP-47 | Technical clarification | AI-RECOMMENDATION | Medium | REQ-077, 093, 108-110 | Payor Mapping has 291 exact duplicate rows, 215 plan descriptions used in more than one row, 19 rows with Payor Portal "NULL", and combined values such as "ABILITY, AVAILITY". | Can duplicates be removed at the source? | De-duplicate at load; treat "NULL" as not identified; try combined portals in the order listed. |

## Summary

| Class | Count |
|---|---|
| Class (open findings, r2) | Count |
|---|---|
| Critical blocker | 7 (GAP-02, 03, 05, 39, 40, 41, 42) |
| Business clarification | 22 |
| Technical clarification | 12 |
| Known limitation | 2 |
| Recommended enhancement | 2 |
| Resolved / deferred | 2 (GAP-01 resolved, GAP-04 deferred) |
| **Total** | **47** (42 BRD-GAP, 5 AI-RECOMMENDATION) |

**Top blockers (r2):** GAP-39 (Payor Mapping key columns empty), GAP-42 (Service Area sheet missing), GAP-40 (no paediatric plan flag), GAP-41 (Facility routing columns empty), GAP-02 (Medicare bypass skips the existing-patient check), GAP-05 (flow diagram).

## Questions to send, grouped by owner

**Intake / business (Moira Behrman, Jaimie Miller, Jennifer Williams)**
1. ~~What is the Approval Required List?~~ Resolved. Please correct the 3 future DOBs. (GAP-46)
2. Should the Medicare bypass check Kantime existing patients first? (GAP-02)
3. Is an 18-year-old paediatric or adult? (GAP-06)
4. Is this build Careport only? (GAP-07)
5. What is the patient match rule, and how should duplicates be handled? (GAP-08, 09)
6. What is the Ask response SLA and escalation? (GAP-10)
7. Should Medicare skip the keyword and census checks? (GAP-12)
8. How should partial order acceptance work, how are inactive vs not eligible defined, and how should non-Medicare secondaries be handled? (GAP-13, 14, 15)
9. Is Insurance master row order a priority; what are the keyword matching rules; and what are the Mercy matching examples? (GAP-16, 17, 19)
10. What are the Careport dropdown values, and the non-admit reason for No UHC? (GAP-18, 23)
11. How should unmapped manual-decline comments, EPIC, and the "Interested" follow-up be handled? (GAP-21, 22)
12. Send the approved flow diagram and the Service Area sheet. (GAP-05, 42)

**Master data owner (CareFlo admin)**
12a. Re-export Payor Mapping with Agency, Medicare, Commercial, Identifier, Contract, EMR Payor Name and a Paediatric flag. (GAP-39, 40, 47)
12b. Re-export Facility Master with the routing columns filled, and add the dummy facility. (GAP-41)
12c. Fix the Restricted Facility master and add the ProMedica and Mercy location rows. (GAP-45)
12d. Re-export the Exception Keywords Master with Line of Business values, and review the short keywords. (GAP-43, 44)

**Insurance team**
13. Payer Portal BRD: when will it be available? (deferred, GAP-04). Define inactive and future-effective coverage. (GAP-14)

**IT / security (client)**
14. Bot accounts, OTP mailbox, password rotation and concurrent sessions. (GAP-25)
15. Test referrals or a sandbox for Careport and the payer portals. (GAP-26)
16. PHI rules for logs, email and SharePoint. (GAP-33)

**AutomationEdge engineering**
17. Runner count and queue split for the SLA; extraction confidence; partial-success recovery; retry policy; county API provider; CareFlo data contract. (GAP-27-32, 34)

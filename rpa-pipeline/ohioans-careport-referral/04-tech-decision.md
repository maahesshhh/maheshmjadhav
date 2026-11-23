# Technology decision: Ohioans HHC Referral Bot (Careport) migration to the new CareFlo platform

Inputs: `02-requirements.json` r2 (112 requirements), `03-gap-register.md` r2 (47 findings). Pipeline gate approved 2026-11-23 with data blocks GAP-39 to GAP-42 still open.

## 1. Recommendation

**Recommended: Hybrid. Process Studio for orchestration and UI, a Drools rule engine for the decisions, and one small Java utility JAR.**

| Layer | Technology | Covers |
|---|---|---|
| Orchestration, queues, CareFlo Asks/Alerts/logging, email, SharePoint, master loading | AutomationEdge Process Studio | REQ-001-006, 012, 018-022, 070, 101, 107-112 |
| Referral portal and EMR UI (Careport, Kantime), payer-portal login | Process Studio web automation | REQ-001-005, 014, 018-028, 058, 071-090 |
| Accept/decline/review decisions (Medicare bypass, service area, age/plan, keywords, payer mapping, UHC/Humana, agency, census, restricted facility, portal outcome, identifier) | Drools rule engine (DRL), called from the workflow | REQ-007-010, 029-069, 092-096 |
| Name/DOB matching, text normalisation, whole-word keyword search, identifier prefix match, master data checks | Java JAR (or UDJC) | REQ-013, 023, 047, 093-096, 107, 112 |
| PDF extraction (H&P, CRF, F2F, DC Order, AVS, Referral) | DocEdge | REQ-011, 015, 017 |

**Runner-up: the same hybrid, but with Playwright for the Careport UI.** Switch to it if Careport turns out to be unreliable with Process Studio web steps (see section 7).

## 2. Scoring

Weights: Reliability 25%, Development effort 15%, Performance 15%, Maintenance 15%, Cost 15%, Ecosystem 10%, Scalability 5%.
Scores run from 1 (poor) to 5 (best); a high score for effort or cost means less of it.

| Criterion (weight) | A. Process Studio only | B. Java only | C. Playwright only | **D. Hybrid PS + Drools + JAR** | E. Hybrid PS + Drools + Playwright |
|---|---|---|---|---|---|
| Reliability (25%) | 3 | 4 | 4 | **4** | 4 |
| Development effort (15%) | 4 | 2 | 3 | **3** | 2 |
| Performance (15%) | 3 | 4 | 4 | **4** | 4 |
| Maintenance (15%) | 2 | 3 | 3 | **5** | 4 |
| Cost (15%) | 5 | 4 | 3 | **4** | 3 |
| Ecosystem (10%) | 5 | 3 | 2 | **5** | 3 |
| Scalability (5%) | 3 | 4 | 4 | **4** | 4 |
| **Weighted total** | 3.50 | 3.45 | 3.35 | **4.10** | 3.45 |

Why each score:
- **A. Process Studio only.**
  - Reliability 3: about 45 interacting decision branches built as workflow hops are fragile.
  - Effort 4: this is the team's native tool.
  - Performance 3: branch-heavy workflows are slow to evaluate and debug.
  - Maintenance 2: BRD v1.25 has 25 versions, mostly rule changes, and every one would mean editing workflows.
  - Cost 5: no new licences.
  - Ecosystem 5: native to AutomationEdge and CareFlo.
  - Scalability 3.
- **B. Java only.**
  - Reliability 4: strong for logic.
  - Effort 2: CareFlo integration, Asks, email and SharePoint would all be rebuilt by hand.
  - Performance 4.
  - Maintenance 3: rule changes need Java releases.
  - Cost 4.
  - Ecosystem 3: ends up as a JAR called from AutomationEdge anyway.
  - Scalability 4.
- **C. Playwright only.**
  - Reliability 4: resilient locators. Whether Careport and Kantime are dynamic web apps is unknown, because only screenshots were supplied.
  - Effort 3.
  - Performance 4.
  - Maintenance 3: rules still have to live somewhere.
  - Cost 3: needs a separate Node or Python runtime and its own operations on the agents.
  - Ecosystem 2: outside AutomationEdge and CareFlo.
  - Scalability 4.
- **D. Hybrid (recommended).**
  - Reliability 4: rules are deterministic and unit-testable, and the UI stays on proven AutomationEdge steps.
  - Effort 3: writing the DRL adds effort but removes workflow branching.
  - Performance 4: rule calls take milliseconds, which helps the 1-hour SLA and the Medicare bypass.
  - Maintenance 5: frequent rule changes are made in the DRL without touching the UI bots.
  - Cost 4: a rule-engine runtime is needed; check whether CareFlo already provides one.
  - Ecosystem 5.
  - Scalability 4: decision and chart-creation queues can scale on separate runners.
- **E. Hybrid with Playwright.**
  - Same as D, but effort 2, maintenance 4, cost 3 and ecosystem 3, because it adds a second UI stack with no evidence yet that it's needed.

## 3. Reasons, tied to the requirements

1. **The rules change often.** v1.0 to v1.25 is mostly rule changes: PO Box (v1.13/1.16), PPS (v1.14), No UHC/Humana (v1.17), non-admit (v1.18), paediatric (v1.19), census and referral source (v1.22/1.23), the county fallback (v1.25). A DRL keeps those changes away from the UI bots (GAP-37).
2. **The decisions are table-shaped.** Service area, payer contract colour per order, restricted facility colour and agency-appropriate plan are all lookups against masters followed by a fixed outcome (REQ-037, 052-063). These are a natural fit for Drools facts and rules.
3. **Speed matters.** Portals are first-come first-served and the SLA is 1 hour (REQ-102). The Medicare bypass (REQ-008) exists only to save time. Separating the decision from chart creation (GAP-27) is easier when the decision is one rule call.
4. **Matching logic is where errors hide.** Name+DOB matching (GAP-08, 46), whole-word keywords with short tokens (GAP-43), identifier prefixes (REQ-096) and non-breaking spaces in masters (REQ-107) need code with unit tests, not workflow steps.
5. **The ecosystem.** The BRD is written for AutomationEdge delivery. The masters are CareFlo exports, and Asks and Alerts live in CareFlo (ASM-005). Process Studio is the lowest-risk home for orchestration and for the Kantime and Careport UI.

## 4. Reuse

None of these were supplied or confirmed, so none are counted in the estimate:
- The BRD refers to the existing bot's steps for chart sections and document upload ("same as existing bot configured", sections 5.18, 5.20, 5.21, 5.29). If you can share the current Ohioans referral bot workflows, the Kantime chart, readmit, non-admit and upload work (about 70-110 hours) could largely be reused.
- **Your Drools BRD-to-rules skill** can produce the first DRL from `02-requirements.json`.

## 5. Estimate (development hours, an estimate only)

| Work package | Hours |
|---|---|
| Careport: login (OTP), monitoring, masked and status handling, screen extraction, responses including multi-branch | 60-90 |
| Careport document combine, print fallback, SharePoint, naming | 20-30 |
| DocEdge extraction setup (7 document types, services/orders, discharge note) | 40-70 |
| Kantime report download and existing-patient lookup | 16-24 |
| Existing-patient flows (Intake, Active, Transfer, PSOC) with mismatch emails | 24-36 |
| Kantime new chart, readmit, non-admit, eligibility, uploads, team/marketer | 70-110 |
| Drools rules (about 45 rules) with unit tests | 50-80 |
| Java JAR: normalisation, matching, keywords, identifiers, master checks | 24-40 |
| County lookup API with zip-only fallback | 8-16 |
| Payer portal login and outcome routing (8 portals, login and patient search only) | 40-64 |
| CareFlo Asks, Alerts, status logging, manual decline, "Interested" | 40-64 |
| Error handling, retries, queues, partial-success recovery | 24-40 |
| Test support (SIT, supervised production pilot; Careport has no UAT) | 60-100 |
| **Total** | **about 480-760 hours** |

This assumes:
- Only Careport is in scope (ASM-013).
- The per-portal eligibility steps are excluded, since they wait for the Payer Portal BRD.
- No reuse of the existing bot.
- Filled masters arrive before rules testing.
- One AutomationEdge developer pair, plus a DocEdge specialist part-time.

**Not included:** the Payer Portal BRD work (likely 15-30 hours per portal once it's known), other referral portals, and rework if the approved flow diagram differs from the BRD.

## 6. Infrastructure, licences, maintenance and risks

**Infrastructure and licences**
- **Server and agents:** an AutomationEdge server, and at least 2 Windows agents with Chrome, one for the decision/response queue and one for chart creation.
- **Verify, don't assume:**
  - that the AutomationEdge web activities can run headless on these agents;
  - whether a Linux or container runtime is supported.
  - Plan for interactive Windows sessions until both are confirmed.
- **Rule engine:** a Drools runtime. Confirm whether CareFlo's rule engine can host this DRL, or whether a separate service or embedded KIE JAR is needed.
- **Other services:**
  - DocEdge licence (pages per month, scanned documents);
  - Google Geocoding/Maps API key with billing (or confirm Randymajors is allowed);
  - SharePoint access (Graph API or a mapped library);
  - SMTP/Exchange for notifications;
  - a mailbox the bot can read for OTP codes.
- **Accounts:** dedicated bot accounts for Careport, Kantime and the 8 payer portals, plus a password-rotation process (GAP-25).

**Maintenance**
- Rule changes go to the DRL plus unit tests; they don't need a workflow redeploy if the engine loads DRL at runtime.
- UI changes in Careport or Kantime need selector fixes; keep the selectors in one repository and run a daily smoke test (GAP-36).
- Master data is owned by CareFlo admins; the load-time checks (REQ-112) catch empty columns like the ones found in r2.

**Risks and dependencies**
| Risk | Effect | Mitigation |
|---|---|---|
| Payor Mapping, Facility and Service Area data empty or missing (GAP-39 to 42) | Rules can be built but not tested | Request re-exports now; test with synthetic masters |
| No UAT for Careport or payer portals (GAP-26) | Testing on real referrals | Dry-run mode, then a supervised pilot |
| OTP by email (GAP-25) | Login failures stop intake | Dedicated mailbox and alert on failure |
| Payer Portal BRD missing (GAP-04) | Eligibility outcome only; notes and eligibility fields incomplete | Separate work package |
| Rule engine hosting unknown | Architecture change | Confirm in the solution design; fallback is an embedded KIE JAR called from Process Studio |
| Careport UI behaviour unknown (only screenshots) | Unreliable steps | A 2-day Careport spike before full build (section 7) |

## 7. What would change this recommendation

- **Careport or Kantime is unreliable with Process Studio web steps** (shadow DOM, heavy client rendering, frequent popups) in a 2-day spike: switch that portal to Playwright (option E).
- **No rule engine is available or allowed:** move the rules into a Java JAR with a rules table (option B for logic), keeping Process Studio for orchestration.
- **Careport offers an API or integration feed** (unknown; not in the material): use it for discovery and extraction instead of the UI. This would improve reliability and speed.
- **The Payer Portal BRD brings complex eligibility scraping** across 8 portals: consider Java or Playwright per portal, called from Process Studio.
- **The existing Ohioans bot workflows are shared:** the estimate drops by about 70-110 hours and the case for Process Studio gets stronger.

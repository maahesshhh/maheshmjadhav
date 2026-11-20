# Requirements: Ohioans HomeHealth Care - Referral Bot (Careport) migration to new CareFlo platform

Source: BRD v1.25 (no flowchart, samples or reference code supplied). Generated 2026-11-20.

## Source authority
1. **BRD v1.25 body (sections 5.x)**: Wins over the document history notes
2. **BRD document history**: Used only when the body does not cover the change
3. **Approved To-Be flow diagram**: Would win over BRD prose by default, but not supplied (MIS-001)

## Scope
**In scope:** BRD sections 5.1-5.31 for the Careport referral portal; Kantime EMR; Payer portal login and outcome routing

**Excluded:**
- Other referral portals' UI: Not described (CON-008)
- Coordination team work: Not for automation
- Per-portal eligibility steps: In the separate Payer Portal BRD

## Counts (from validator)
- Requirements: **106** (104 in scope, 2 excluded)
- Stated / assumed: 105 / 1
- Blocked by missing input: 4
- Conflicts: 12; assumptions: 16 (High 4, Medium 10, Low 2); missing inputs: 10
- Source-reference coverage: 100.0%

## Requirements by category

### Referral Intake (7)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-001 | Log in to referral portals | process | 5.1.1 | Extracted |
| REQ-002 | Monitor new referrals continuously | process | 5.1.2; 2 (Process Summary) | Extracted |
| REQ-003 | Fetch referral data from portal screen | process | 5.1.3-5.1.4 | Resolved-conflict |
| REQ-004 | Masked Careport referral handling | rule | 5.1.5 | Extracted |
| REQ-005 | Non 'Not Placed' status goes to manual | error-handling | 5.31 / 8.4 Post Sign off observations | Extracted |
| REQ-105 | Other referral portals' UI | process | 2 (Brief Description); 5.21.2 | Resolved-conflict / excluded |
| REQ-106 | Coordination team work | process | 2 (No. of Users) | Extracted / excluded |

### Referral Portal Response (1)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-006 | Same response for all branch sites | rule | 5.1 (all response tables) | Extracted |

### Medicare Bypass (4)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-007 | Medicare in-area patient on Approval Required list | rule | 5.1.7 | Extracted |
| REQ-008 | Medicare in-area patient auto-accepted without PDF extraction | rule | 5.1.8 | Resolved-conflict |
| REQ-009 | Medicare out-of-area decline | rule | 5.1.9 | Extracted |
| REQ-010 | Non-Medicare or missing data goes to full path | rule | 5.1.10 | Extracted |

### Document Extraction (1)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-011 | Extract remaining details from referral PDFs | process | 5.1.11-5.1.12 | Extracted |

### Exception Handling (4)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-012 | Basic Info Missing Ask | error-handling | 5.1.13 | Extracted |
| REQ-040 | PO Box address flags manual + notification | rule | 0.1 Document History v1.13 (12.6) | Resolved-conflict |
| REQ-070 | Intake Review Ask when any flag set | rule | 5.13.2; 5.30.6.2.1 | Extracted |
| REQ-097 | Email for manual processing when identifier missing | error-handling | 0.1 Document History v1.11 (5.1.16.2) | Extracted |

### Patient Eligibility (6)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-013 | Approval Required list check (flag 2) | rule | 5.1.14 | Blocked-missing-input |
| REQ-041 | Paediatric patient without paediatric plan: decline | rule | 5.13.1 | Resolved-conflict |
| REQ-042 | Paediatric plan + exception keyword -> review | rule | 5.13.2 | Resolved-conflict |
| REQ-043 | Paediatric plan, no keyword: auto accept | rule | 5.13.3; Doc History v1.19 | Resolved-conflict |
| REQ-044 | Adult with paediatric plan: decline | rule | 5.14 | Resolved-conflict |
| REQ-045 | Age boundary for paediatric vs adult | rule | 5.13 / 5.14 / 5.24 / 5.31 | Assumed |

### Data Capture (4)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-014 | Capture 25 referral fields from Careport | data-mapping | 5.2 (table, S no 1-25) | Extracted |
| REQ-015 | Extract Home Health Order (services) | data-mapping | 5.2 (S no 19) | Extracted |
| REQ-016 | Classify referral source as Facility or Physician | rule | 5.2 (S no 22); 5.24 Referral Source table | Extracted |
| REQ-017 | Discharge note / SOC date drives Discharging status | data-mapping | 5.2 (S no 20; Discharge Notes) | Extracted |

### Document Handling (3)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-018 | Combine Careport documents into one PDF | process | 5.3.1-5.3.5 | Extracted |
| REQ-019 | Print referral when no documents | process | 5.3.6-5.3.10 | Extracted |
| REQ-020 | Kantime per-document upload naming | rule | 5.3.6; 5.3.11-5.3.12 | Resolved-conflict |

### Kantime Lookup (3)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-021 | Download Referral Management-Intake list 4x daily | integration | 5.4.1-5.4.3 | Extracted |
| REQ-022 | Download Client Intake list every 30 minutes | integration | 5.4.4-5.4.6 | Extracted |
| REQ-023 | Route by Kantime report match | rule | 5.4.7-5.4.10 | Extracted |

### Existing Patient (6)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-024 | Client Intake list / Intake status handling | rule | 5.5; 5.6 | Extracted |
| REQ-025 | Active / Recert Due / EOE handling | rule | 5.7 | Extracted |
| REQ-026 | Transfer / On Hold handling | rule | 5.8 | Extracted |
| REQ-027 | Compare Careport vs Kantime for mismatches | rule | 5.7.3; 5.8.3 | Extracted |
| REQ-028 | PSOC handling | rule | 5.9 | Extracted |
| REQ-029 | Do not decline/flag active-type statuses | rule | 0.1 Document History v1.12 (Section 11.6) | Extracted |

### Mandatory Data (3)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-030 | Mandatory EMR Ask for missing payer/policy | error-handling | 5.10.1; 5.11.1 | Extracted |
| REQ-031 | Medicare: missing address Ask | error-handling | 5.10.2 | Extracted |
| REQ-036 | Non-Medicare: missing or PO Box address Ask | error-handling | 5.11.2-5.11.3 | Extracted |

### Medicare Path (4)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-032 | Medicare PO Box address: accept and ask for physical address | rule | 5.10.3; Doc History v1.16 | Extracted |
| REQ-033 | Medicare physical address in-area accept | rule | 5.10.4 | Extracted |
| REQ-034 | Medicare physical address out-of-area decline | rule | 5.10.5 | Extracted |
| REQ-035 | Medicare PPS plans auto-acceptance | rule | 0.1 Document History v1.14, v1.16 | Extracted |

### Service Area (3)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-037 | Non-Medicare zip+county service area check | rule | 5.11.4-5.11.5 | Extracted |
| REQ-038 | Get county from patient address | integration | 5.12.1-5.12.4 | Resolved-conflict |
| REQ-039 | County API no value: zip-only fallback | error-handling | 5.12 Exception scenario; Doc History v1.25 | Extracted |

### Exception Keywords (2)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-046 | Read exception keyword list daily | integration | 5.15.1 | Extracted |
| REQ-047 | Keyword search in comments and PDFs | rule | 5.15.2-5.15.5 | Extracted |

### Payer Identification (12)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-048 | Dummy payor in insurance master: decline | rule | 5.16.1 | Extracted |
| REQ-049 | Payor found (non-dummy): continue to UHC/Humana check | rule | 5.16.2 | Extracted |
| REQ-050 | Payor not found: identifier match path | rule | 5.16.3 | Resolved-conflict |
| REQ-051 | Define Humana and UHC plan descriptions | rule | 5.17 | Extracted |
| REQ-052 | No UHC/Humana zip limitation decline | rule | 5.18 | Resolved-conflict |
| REQ-053 | Agency appropriate plan check | rule | 5.19 | Extracted |
| REQ-092 | Plan in one row: agency match | rule | 5.30.1-5.30.3 | Extracted |
| REQ-093 | Plan in many rows: agency then identifier | rule | 5.30.4-5.30.6.3 | Resolved-conflict |
| REQ-094 | Plan not in master: identifier search by agency | rule | 5.30 (Plan Description Not found) | Extracted |
| REQ-095 | Identifier Ask outcomes | rule | 5.30.6.2.1 | Extracted |
| REQ-096 | Identifier rule = address state + prefix | rule | 5.31 / 8.4 Post Sign off observations; 5.30 examples | Extracted |
| REQ-098 | Insurance best match | rule | 0.1 Document History v1.23 | Blocked-missing-input |

### Capacity (4)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-054 | Staffing availability per order | rule | 5.20.1 | Extracted |
| REQ-055 | Census at maximum capacity per payer colour | rule | 5.20.2 | Extracted |
| REQ-056 | Decline only when no order can be accepted | rule | 5.20.3-5.20.10 | Extracted |
| REQ-057 | Census decline creates Non-Admit chart | rule | 5.20.11 | Resolved-conflict |

### EMR - Kantime (18)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-058 | Non-Admit chart creation steps | process | 5.18 i-vii; 5.20; 5.21; 5.29 | Extracted |
| REQ-071 | Readmit: APPROVAL REQUIRED client alert | rule | 5.23.1 | Extracted |
| REQ-072 | Readmit chart steps | process | 5.23.2-5.23.9 | Extracted |
| REQ-073 | Readmit when duplicate check finds Non-Admit | rule | 0.1 Document History v1.15 | Extracted |
| REQ-074 | Location from service area; Monroe override | data-mapping | 5.24 New Intake table row 1; Doc History v1.12 | Extracted |
| REQ-075 | Line of Business by age | data-mapping | 5.24 New Intake table row 2 | Resolved-conflict |
| REQ-076 | Patient name camel case | data-mapping | 5.24 rows 3-4 | Extracted |
| REQ-077 | Kantime payer from insurance sheet | data-mapping | 5.24 row 5; Doc History v1.2 | Extracted |
| REQ-078 | Patient information section | data-mapping | 5.24 Patient Information table | Extracted |
| REQ-079 | Non-Medicare Insurance ID | data-mapping | 5.24; Doc History v1.7 | Extracted |
| REQ-080 | Secondary Medicare payer | data-mapping | 5.24; Doc History v1.9 | Extracted |
| REQ-081 | Referral source section | data-mapping | 5.24 Referral Source table; 5.31 / 8.4 | Extracted |
| REQ-082 | Address from payer portal | data-mapping | 5.24 Address table | Resolved-conflict |
| REQ-083 | Other info, emergency contact, ACO | data-mapping | 5.24 Other Info / Emergency / ACO tables | Extracted |
| REQ-085 | Eligibility title by plan type | rule | 5.25.1-5.25.6 | Blocked-missing-input |
| REQ-086 | Commercial notes: coverage, deductible, OOP | data-mapping | 5.25.7-5.25.14 | Resolved-conflict |
| REQ-087 | Effective period and eligibility status | process | 5.25.15-5.25.21 | Extracted |
| REQ-088 | Team (CC1) update | data-mapping | 5.26 | Extracted |

### Referral Source Exception (6)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-059 | Restricted facility check applicability | rule | 5.21.1-5.21.3 | Extracted |
| REQ-060 | Restricted facility colour match decline | rule | 5.21.4-5.21.8 | Resolved-conflict |
| REQ-061 | Re-read Restricted Facility sheet | nfr | 5.21.1 | Extracted |
| REQ-062 | ProMedica location matching | rule | 5.21 (Promedica); Doc History v1.13 | Extracted |
| REQ-063 | Bon Secours Mercy Health location matching | rule | 5.21 (Mercy Health) | Extracted |
| REQ-100 | Decline by referral source after census check | rule | 0.1 Document History v1.22, v1.23 | Extracted |

### Payer Eligibility (6)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-064 | Log in to identified payer portal | integration | 5.22.1-5.22.2 | Resolved-conflict |
| REQ-065 | Patient not found in payer portal | rule | 5.22.3 | Resolved-conflict |
| REQ-066 | Primary payer eligibility per Payer Portal BRD | integration | 5.22.4 | Blocked-missing-input |
| REQ-067 | Eligible: accept and create/readmit chart | rule | 5.22.5 | Extracted |
| REQ-068 | Inactive payer -> review flag | rule | 5.22.6 | Resolved-conflict |
| REQ-069 | Not eligible: decline, no chart | rule | 5.22.7 | Extracted |

### Document Upload (1)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-084 | Upload referral and payer documents | process | 5.24 upload steps; Doc History v1.4 | Resolved-conflict |

### Notification (2)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-089 | Marketer name and email | data-mapping | 5.27 | Extracted |
| REQ-090 | Team email ID | data-mapping | 5.28 | Extracted |

### Manual Decline (1)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-091 | Apply intake team decline from CareFlo | rule | 5.29 | Extracted |

### CareFlo App (1)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-099 | 'Interested' action in CareFlo | rule | 5.31 | Extracted |

### Logging (1)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-101 | Log all actions to CareFlo DB | nfr | 2 (Process Purpose; Type of Output); 5.20 | Extracted |

### Non-Functional (3)
| ID | Title | Type | Section | Status |
|---|---|---|---|---|
| REQ-102 | Respond within 1 hour; speed matters | nfr | 2 (TAT/SLA); 5.1 title | Extracted |
| REQ-103 | Volume 60-90 per day | nfr | 2 (Volume; Frequency) | Extracted |
| REQ-104 | Applications and access | nfr | 3 (embedded applications sheet) | Extracted |

## Conflicts
| ID | Topic | Resolution | Risk | Affects |
|---|---|---|---|---|
| CON-001 | Broken cross-reference for payor not found | Route to 5.30 (title matches the described step). | Low | REQ-050 |
| CON-002 | Restricted facility text duplicated and truncated | Apply the same colour-match rule to Mercy Health. | Low | REQ-060, REQ-063 |
| CON-003 | Decline Response vocabulary differs between sections | Use each section's values as written; confirm the actual Careport dropdown values. | Medium | REQ-041, REQ-044, REQ-060 |
| CON-004 | County lookup source | Use Google APIs (latest body text); keep the provider configurable. | Medium | REQ-038 |
| CON-005 | Age 18 boundary | Paediatric = under 18 completed years; 18 and above = adult (ASM-002). | High | REQ-041, REQ-042, REQ-043, REQ-044, REQ-045, REQ-075 |
| CON-006 | Kantime document naming | Use the 5.3 convention for referral packet documents. | Medium | REQ-020, REQ-084 |
| CON-007 | Payer portal list | In scope: the 8 listed portals plus Cigna for notes; Inovalon treated as not in scope until confirmed. | Medium | REQ-064, REQ-086 |
| CON-008 | Referral portals in scope | Careport is the only referral portal UI in scope; business rules are written portal-agnostic. | High | REQ-003, REQ-105 |
| CON-009 | Intake Review Flag8 reused for different reasons | One Flag8 with a reason text; the Ask shows the reason. | Medium | REQ-065, REQ-068, REQ-093 |
| CON-010 | Non-Admit reason for No UHC/Humana decline | Use 'Census at maximum capacity' as written; confirm. | Low | REQ-052, REQ-057 |
| CON-012 | Medicare bypass skips Kantime report lookup | Bypass accepts directly; chart creation uses Kantime Check Duplicate and readmits when the patient exists (ASM-014). | High | REQ-008 |
| CON-013 | PO Box rules and address source | Medicare PO Box -> accept as 5.10.3; non-Medicare PO Box -> Mandatory EMR Ask (5.11.3); use referral address when no portal check was done (ASM-012). | Medium | REQ-040, REQ-082 |

## Assumptions (High risk first)
| ID | Statement | Risk | If wrong | Affects |
|---|---|---|---|---|
| ASM-001 | The 'Approval Required List' is a client-maintained list (sheet or Kantime client alert) checked by patient name + DOB. | High | Flag1/Flag2 logic and the Ask volume change. | REQ-007, REQ-013 |
| ASM-002 | Paediatric = under 18 completed years; 18 and above = adult. | High | 18-year-olds get the wrong plan rule and Line of Business. | REQ-045, REQ-041 |
| ASM-013 | Only Careport referral UI is in scope for this build. | High | Scope and effort grow significantly. | REQ-105 |
| ASM-014 | Medicare bypass chart creation runs Kantime Check Duplicate and readmits an existing patient. | High | Duplicate charts for existing patients. | REQ-008 |
| ASM-003 | 'Medicare' means the Insurance master Medicare column = Yes (Row 2 plan / MBI). | Medium | Wrong referrals take the bypass path. | REQ-007, REQ-008 |
| ASM-005 | Asks and Alerts are raised in the CareFlo app and answered there by the intake team. | Medium | Need email/Teams integration. | REQ-070 |
| ASM-007 | The census rule generalises to any number of orders: decline only if no order can be accepted. | Medium | Wrong outcome with 4+ disciplines. | REQ-056 |
| ASM-008 | The keyword list is read at least once a day and matching is case-insensitive whole-word. | Medium | False positives/negatives on Flag6. | REQ-046 |
| ASM-009 | Mercy partial match = normalised 'contains' match after removing spaces/punctuation. | Medium | Restricted facilities missed or over-matched. | REQ-063 |
| ASM-010 | Up to 30 minutes (Client Intake) and 5 hours (RM-Intake) stale data is accepted. | Medium | Recent patients treated as new -> duplicate charts. | REQ-021, REQ-022 |
| ASM-011 | 'Inactive' = coverage terminated (Flag8 review); 'not eligible' = active but plan not accepted/no HH benefit (decline). | Medium | Wrong decline vs review split. | REQ-068, REQ-069 |
| ASM-012 | When no payer portal check was done, the Kantime address comes from the referral portal. | Medium | Address left blank or wrong. | REQ-082 |
| ASM-016 | Medicare PPS auto-acceptance is already covered by the Medicare rules in 5.1/5.10. | Medium | A PPS rule is missing. | REQ-035 |
| ASM-017 | 'Paediatric Plan (Purple)' is a colour/flag column in the Insurance master. | Medium | Plan check needs another source. | REQ-041 |
| ASM-006 | Intake Review Flags 3, 4 and 5 are not used in this process. | Low | Missing review triggers. | REQ-070 |
| ASM-015 | 'Update CareFlo app' means writing status and decision to the CareFlo database. | Low | Schema rework. | REQ-101 |

## Missing inputs
| ID | Input | Referenced in | Blocks | Working assumption |
|---|---|---|---|---|
| MIS-001 | To-Be process flow diagram (approved by Jaimie, v1.24) | BRD 4; Doc History v1.24 | REQ-003, REQ-008, REQ-023 | BRD prose is the only authority; it may differ from the approved diagram. |
| MIS-002 | Service Area mapping sheet (Insurance and Service Areas Google sheet) | BRD 5.12, 5.15, 5.18, 5.20, 5.23, 5.24 | REQ-037, REQ-046, REQ-052, REQ-054, REQ-055, REQ-074 | Columns as described: A Zip, B County+State, G Location, H Limitation, L Exceptions, order columns with x/colours, Team. |
| MIS-003 | Insurance plan master | BRD 5.16-5.19, 5.24, 5.25, 5.30 | REQ-048, REQ-051, REQ-053, REQ-093, REQ-085 | Columns: Plan description, Kantime payer, Identifiers (C), Payer portal, Payer contract colour, Associated agencies (F), Commercial, Medicare, Paediatric flag. |
| MIS-004 | Referral Source mapping sheet | BRD 5.24, 5.26-5.28 | REQ-081, REQ-088, REQ-089, REQ-090 | Columns F facility, G Kantime facility, H/I sales person, K email, CC1, Admission DL, Admission Source. |
| MIS-005 | Restricted Facility / Referral Source Exception sheet | BRD 5.21 | REQ-060, REQ-062, REQ-063 | Format as in 5.21 tables. |
| MIS-006 | Payer Portal BRD | BRD 5.22.4 | REQ-066, REQ-086 | Separate BRD covers per-portal eligibility steps. |
| MIS-007 | Approval Required List | BRD 5.1.7, 5.1.14 | REQ-007, REQ-013 | ASM-001. |
| MIS-008 | Ask/Alert catalogue (texts, recipients, channels) | BRD throughout | REQ-070, REQ-004 | CareFlo app standard Asks/Alerts. |
| MIS-009 | Eligibility note templates (Commercial/Medicaid/Medicare AA) | BRD 5.25 | REQ-085, REQ-086 | Only in screenshots; to be transcribed. |
| MIS-010 | 6 Months Plan description from Careport; Physician Library; Facility Library | BRD 0.2 Related Documentation | - | Reference only. |

## Divergences from reference code
None (no reference code supplied).

## Technology hints (for tech selection)
- Careport, Kantime and 8 payer portals are Chrome web apps; OTP/password reset needed for some
- PDF extraction via DocEdge (digital and scanned)
- County lookup via Google APIs or Randymajors.org
- SharePoint document storage
- Email notifications
- Masters in Google Sheets / Excel
- CareFlo app database for Asks, Alerts and logging
- No UAT for Careport or payer portals
- Speed critical: 1-hour SLA, first-come first-served portals

Full detail of every requirement: `02-requirements.json`.

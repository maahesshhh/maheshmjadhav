# Traceability matrix

Result = Not run for every case (Mode A).

| Requirement | Title | Scope | Test cases | Result | Evidence |
|---|---|---|---|---|---|
| REQ-001 | Log in to referral portals | in_scope | TC-001, TC-002, TC-179 | Not run | |
| REQ-002 | Monitor new referrals continuously | in_scope | TC-003, TC-004 | Not run | |
| REQ-003 | Fetch referral data from portal screen | in_scope | TC-005 | Not run | |
| REQ-004 | Masked Careport referral handling | in_scope | TC-006, TC-007 | Not run | |
| REQ-005 | Non 'Not Placed' status goes to manual | in_scope | TC-008, TC-009 | Not run | |
| REQ-006 | Same response for all branch sites | in_scope | TC-010 | Not run | |
| REQ-007 | Medicare in-area patient on Approval Required list | in_scope | TC-011 | Not run | |
| REQ-008 | Medicare in-area patient auto-accepted without PDF extraction | in_scope | TC-012, TC-013 | Not run | |
| REQ-009 | Medicare out-of-area decline | in_scope | TC-014 | Not run | |
| REQ-010 | Non-Medicare or missing data goes to full path | in_scope | TC-015, TC-016 | Not run | |
| REQ-011 | Extract remaining details from referral PDFs | in_scope | TC-017, TC-018 | Not run | |
| REQ-012 | Basic Info Missing Ask | in_scope | TC-019, TC-020 | Not run | |
| REQ-013 | Approval Required list check (flag 2) | in_scope | TC-011, TC-021, TC-022, TC-023 | Not run | |
| REQ-014 | Capture 25 referral fields from Careport | in_scope | TC-024, TC-025 | Not run | |
| REQ-015 | Extract Home Health Order (services) | in_scope | TC-026, TC-027 | Not run | |
| REQ-016 | Classify referral source as Facility or Physician | in_scope | TC-028, TC-029 | Not run | |
| REQ-017 | Discharge note / SOC date drives Discharging status | in_scope | TC-030 | Not run | |
| REQ-018 | Combine Careport documents into one PDF | in_scope | TC-031, TC-032 | Not run | |
| REQ-019 | Print referral when no documents | in_scope | TC-033 | Not run | |
| REQ-020 | Kantime per-document upload naming | in_scope | TC-034, TC-035 | Not run | |
| REQ-021 | Download Referral Management-Intake list 4x daily | in_scope | TC-036 | Not run | |
| REQ-022 | Download Client Intake list every 30 minutes | in_scope | TC-037 | Not run | |
| REQ-023 | Route by Kantime report match | in_scope | TC-038, TC-039, TC-040 | Not run | |
| REQ-024 | Client Intake list / Intake status handling | in_scope | TC-041, TC-042 | Not run | |
| REQ-025 | Active / Recert Due / EOE handling | in_scope | TC-043, TC-044, TC-045 | Not run | |
| REQ-026 | Transfer / On Hold handling | in_scope | TC-046, TC-047 | Not run | |
| REQ-027 | Compare Careport vs Kantime for mismatches | in_scope | TC-044, TC-047 | Not run | |
| REQ-028 | PSOC handling | in_scope | TC-048 | Not run | |
| REQ-029 | Do not decline/flag active-type statuses | in_scope | TC-049 | Not run | |
| REQ-030 | Mandatory EMR Ask for missing payer/policy | in_scope | TC-050 | Not run | |
| REQ-031 | Medicare: missing address Ask | in_scope | TC-051 | Not run | |
| REQ-032 | Medicare PO Box address: accept and ask for physical address | in_scope | TC-052, TC-053 | Not run | |
| REQ-033 | Medicare physical address in-area accept | in_scope | TC-054 | Not run | |
| REQ-034 | Medicare physical address out-of-area decline | in_scope | TC-055 | Not run | |
| REQ-035 | Medicare PPS plans auto-acceptance | in_scope | TC-056 | Not run | |
| REQ-036 | Non-Medicare: missing or PO Box address Ask | in_scope | TC-057 | Not run | |
| REQ-037 | Non-Medicare zip+county service area check | in_scope | TC-058, TC-059 | Not run | |
| REQ-038 | Get county from patient address | in_scope | TC-060 | Not run | |
| REQ-039 | County API no value: zip-only fallback | in_scope | TC-061, TC-062 | Not run | |
| REQ-040 | PO Box address flags manual + notification | in_scope | TC-063 | Not run | |
| REQ-041 | Paediatric patient without paediatric plan: decline | in_scope | TC-064 | Not run | |
| REQ-042 | Paediatric plan + exception keyword -> review | in_scope | TC-065 | Not run | |
| REQ-043 | Paediatric plan, no keyword: auto accept | in_scope | TC-066 | Not run | |
| REQ-044 | Adult with paediatric plan: decline | in_scope | TC-067 | Not run | |
| REQ-045 | Age boundary for paediatric vs adult | in_scope | TC-068, TC-069 | Not run | |
| REQ-046 | Read exception keyword list daily | in_scope | TC-070 | Not run | |
| REQ-047 | Keyword search in comments and PDFs | in_scope | TC-071, TC-072, TC-073, TC-074, TC-075 | Not run | |
| REQ-048 | Dummy payor in insurance master: decline | in_scope | TC-076 | Not run | |
| REQ-049 | Payor found (non-dummy): continue to UHC/Humana check | in_scope | TC-077 | Not run | |
| REQ-050 | Payor not found: identifier match path | in_scope | TC-078 | Not run | |
| REQ-051 | Define Humana and UHC plan descriptions | in_scope | TC-079 | Not run | |
| REQ-052 | No UHC/Humana zip limitation decline | in_scope | TC-080, TC-081, TC-082 | Not run | |
| REQ-053 | Agency appropriate plan check | in_scope | TC-083, TC-084 | Not run | |
| REQ-054 | Staffing availability per order | in_scope | TC-085 | Not run | |
| REQ-055 | Census at maximum capacity per payer colour | in_scope | TC-086 | Not run | |
| REQ-056 | Decline only when no order can be accepted | in_scope | TC-087, TC-088, TC-089, TC-090, TC-091, TC-092, TC-093, TC-094, TC-095, TC-096 | Not run | |
| REQ-057 | Census decline creates Non-Admit chart | in_scope | TC-097 | Not run | |
| REQ-058 | Non-Admit chart creation steps | in_scope | TC-098, TC-099, TC-178 | Not run | |
| REQ-059 | Restricted facility check applicability | in_scope | TC-100, TC-101, TC-102 | Not run | |
| REQ-060 | Restricted facility colour match decline | in_scope | TC-103, TC-104 | Not run | |
| REQ-061 | Re-read Restricted Facility sheet | in_scope | TC-105 | Not run | |
| REQ-062 | ProMedica location matching | in_scope | TC-106 | Not run | |
| REQ-063 | Bon Secours Mercy Health location matching | in_scope | TC-107 | Not run | |
| REQ-064 | Log in to identified payer portal | in_scope | TC-108 | Not run | |
| REQ-065 | Patient not found in payer portal | in_scope | TC-109 | Not run | |
| REQ-066 | Primary payer eligibility per Payer Portal BRD | deferred | TC-110 | Not run | |
| REQ-067 | Eligible: accept and create/readmit chart | in_scope | TC-111, TC-112 | Not run | |
| REQ-068 | Inactive payer -> review flag | in_scope | TC-113, TC-114 | Not run | |
| REQ-069 | Not eligible: decline, no chart | in_scope | TC-115 | Not run | |
| REQ-070 | Intake Review Ask when any flag set | in_scope | TC-116, TC-117 | Not run | |
| REQ-071 | Readmit: APPROVAL REQUIRED client alert | in_scope | TC-118 | Not run | |
| REQ-072 | Readmit chart steps | in_scope | TC-119 | Not run | |
| REQ-073 | Readmit when duplicate check finds Non-Admit | in_scope | TC-120 | Not run | |
| REQ-074 | Location from service area; Monroe override | in_scope | TC-121, TC-122 | Not run | |
| REQ-075 | Line of Business by age | in_scope | TC-123 | Not run | |
| REQ-076 | Patient name camel case | in_scope | TC-124, TC-125 | Not run | |
| REQ-077 | Kantime payer from insurance sheet | in_scope | TC-126 | Not run | |
| REQ-078 | Patient information section | in_scope | TC-127, TC-128 | Not run | |
| REQ-079 | Non-Medicare Insurance ID | in_scope | TC-129 | Not run | |
| REQ-080 | Secondary Medicare payer | in_scope | TC-130, TC-131 | Not run | |
| REQ-081 | Referral source section | in_scope | TC-132, TC-133, TC-134 | Not run | |
| REQ-082 | Address from payer portal | in_scope | TC-135, TC-136 | Not run | |
| REQ-083 | Other info, emergency contact, ACO | in_scope | TC-137, TC-138 | Not run | |
| REQ-084 | Upload referral and payer documents | in_scope | TC-139 | Not run | |
| REQ-085 | Eligibility title by plan type | in_scope | TC-140 | Not run | |
| REQ-086 | Commercial notes: coverage, deductible, OOP | deferred | TC-141 | Not run | |
| REQ-087 | Effective period and eligibility status | in_scope | TC-142 | Not run | |
| REQ-088 | Team (CC1) update | in_scope | TC-143, TC-144 | Not run | |
| REQ-089 | Marketer name and email | in_scope | TC-145, TC-146 | Not run | |
| REQ-090 | Team email ID | in_scope | TC-147 | Not run | |
| REQ-091 | Apply intake team decline from CareFlo | in_scope | TC-148, TC-149 | Not run | |
| REQ-092 | Plan in one row: agency match | in_scope | TC-150, TC-151 | Not run | |
| REQ-093 | Plan in many rows: agency then identifier | in_scope | TC-152, TC-153, TC-154 | Not run | |
| REQ-094 | Plan not in master: identifier search by agency | in_scope | TC-155 | Not run | |
| REQ-095 | Identifier Ask outcomes | in_scope | TC-156, TC-157 | Not run | |
| REQ-096 | Identifier rule = address state + prefix | in_scope | TC-158, TC-159 | Not run | |
| REQ-097 | Email for manual processing when identifier missing | in_scope | TC-160 | Not run | |
| REQ-098 | Insurance best match | in_scope | TC-161 | Not run | |
| REQ-099 | 'Interested' action in CareFlo | in_scope | TC-162 | Not run | |
| REQ-100 | Decline by referral source after census check | in_scope | TC-163 | Not run | |
| REQ-101 | Log all actions to CareFlo DB | in_scope | TC-164, TC-165, TC-176, TC-177 | Not run | |
| REQ-102 | Respond within 1 hour; speed matters | in_scope | TC-166 | Not run | |
| REQ-103 | Volume 60-90 per day | in_scope | TC-167 | Not run | |
| REQ-104 | Applications and access | in_scope | TC-168 | Not run | |
| REQ-105 | Other referral portals' UI | excluded | — (excluded) | Not run | |
| REQ-106 | Coordination team work | excluded | — (excluded) | Not run | |
| REQ-107 | Normalise master text before matching | in_scope | TC-169 | Not run | |
| REQ-108 | Use only Active master rows | in_scope | TC-170 | Not run | |
| REQ-109 | Treat payor portal 'NULL' as not identified | in_scope | TC-171 | Not run | |
| REQ-110 | Handle multi-portal values | in_scope | TC-172 | Not run | |
| REQ-111 | Keyword scope by Line of Business | in_scope | TC-173 | Not run | |
| REQ-112 | Startup master data checks | in_scope | TC-174, TC-175 | Not run | |

## Summary
- Test cases: **179** (by type: {'functional': 100, 'negative': 48, 'boundary': 26, 'regression': 3, 'performance': 2}; by layer: {'UI': 51, 'Workflow': 15, 'Rule': 94, 'DocEdge': 4, 'JAR': 13, 'Integration': 2})
- In-scope requirements covered: 108/108; uncovered: none
- Cases blocked by missing or empty data: 53
- Cases with an open question (no defined expected value): 27
- Deferred/excluded requirements: REQ-066, REQ-086 (deferred, placeholder cases), REQ-105, REQ-106 (excluded, no tests)


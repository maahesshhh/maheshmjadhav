# Pipeline: ohioans-careport-referral
| Stage | Status | Output file | Notes |
|-------|--------|-------------|-------|
| 0 Intake | done | 00-input/ (not committed, contains PHI) | BRD v1.25 + 5 masters (2026-11-23); no flowchart, Service Area sheet, Payer Portal BRD or reference code |
| 1 Prior work | skipped | — | no past projects available to search |
| 2 Requirements | done r2 (2026-11-23) | 02-requirements.json / .md, 02-open_items.md | r2 adds 5 masters: 112 reqs (108 in scope, 2 deferred), 16 conflicts, 17 assumptions (5 High), 10 blocked |
| 3 Gap analysis | done r2 | 03-gap-register.md | 47 findings; 7 open critical blockers; GAP-01 resolved, GAP-04 deferred |
| GATE | approved 2026-11-23 (Proceed) | — | after revise #1. Proceeding with blocked data (GAP-39..42) and assumptions ASM-002, 003, 005, 008, 012, 013, 014, 017; blocked rules stay marked blocked in later stages |
| 4 Tech selection | done | 04-tech-decision.md | Hybrid: Process Studio (orchestration + Careport/Kantime UI) + Drools rules + Java utility JAR + DocEdge; 4.10 vs runner-up 3.45; est. 480-760 h |
| 5 Solution design | pending | | |
| 6 Test design | pending | | |
| 7a AE design | pending | | |
| 7b AE workflow gen | pending | | |
| 7c AE code review | pending | | |

## Assumptions
See 02-requirements.json `assumptions` (ASM-001..017).

## Decisions and overrides

---
name: rpa-project-pipeline
description: Runs an RPA/CareFlo portal automation from BRD to test plan by chaining the team's skills in order - portal-knowledge-finder, brd-requirement-extractor, rpa-gap-analyst, then (only after the user confirms) rpa-tech-selector, rpa-solution-design-writer and rpa-test-designer, and, when Process Studio is chosen, builds the workflow with ae-automation-designer, ae-workflow-gen and ae-code-reviewer. Use this whenever the user shares a BRD, flowchart or referral/portal automation request and wants the end-to-end flow, says "run the pipeline", "BRD to test", "take this BRD through design", or asks to resume a pipeline already in progress. Not for a single stage on its own; use that stage's skill directly.
---

# RPA Project Pipeline (BRD → Test → AE Workflow)

You coordinate other skills. Do not do a stage's work from memory. For each
stage, load the named skill with the Skill tool and follow it.

## Pipeline

| # | Stage | Skill | Input | Output |
|---|-------|-------|-------|--------|
| 0 | Intake | (this skill) | User files | Input inventory |
| 1 | Prior work | `portal-knowledge-finder` | Portal name, BRD summary | Similar projects, reusable components, known issues |
| 2 | Requirements | `brd-requirement-extractor` | BRD, flowchart, samples, reference code, stage 1 output | Requirements package |
| 3 | Gap analysis | `rpa-gap-analyst` | Requirements package | Gap register and clarification list |
| — | **GATE** | (this skill) | Stage 2 and 3 outputs | User's explicit go-ahead |
| 4 | Tech selection | `rpa-tech-selector` | Requirements, gap register, stage 1 reuse | Tech decision with effort, cost and risk |
| 5 | Solution design | `rpa-solution-design-writer` | Requirements, gap register, tech decision | Solution Design Document |
| 6 | Test design | `rpa-test-designer` | Requirements, SDD | Test cases, synthetic data, traceability matrix |
| 7a | AE design | `ae-automation-designer` | SDD, requirements, test cases | Workflow plan (sub-workflows, steps, error handling) |
| 7b | AE workflow generation | `ae-workflow-gen` | Stage 7a plan | Process Studio .psw/.psp files |
| 7c | AE code review | `ae-code-reviewer` | Stage 7b files | HTML audit report |

Stage 7 runs only when stage 4 chose **Process Studio** or a **hybrid**
that includes Process Studio. For a hybrid, it covers only the Process
Studio part.

## Stage 0: Intake

1. List what the user gave you: BRD (docx/pdf), flowchart (pptx), sample
   input/output files, reference DRL or workflow, master files.
2. Ask once for anything essential that is missing. The BRD is required.
   Everything else is optional, so note what's absent and carry on.
3. Create a working folder `rpa-pipeline/<project-name>/` and a
   `pipeline-state.md` file in it (format below).
4. Ask whether to run stage 1. It's recommended but optional. Skip it if
   the user declines or there are no past projects to search.

## Stages 1-3

Run them in order. After each stage:
- Save its output in the working folder (for example `02-requirements.*`,
  `03-gap-register.*`).
- Update `pipeline-state.md`.
- Give the user a 2-3 line summary, then move to the next stage. Don't wait
  for confirmation here unless the stage's own skill asks a question.

## GATE: confirm before tech selection (mandatory)

Never start stage 4 without the user's explicit approval **in this gate**.
An earlier "run everything" or "don't ask me" does not count as approval.
Present:

1. **Requirements:** total count, broken down by type or category, with
   the main scope decisions.
2. **Blocking gaps:** gaps or clarifications that would change the tech
   choice or the design (for example portal access method, volumes, MFA,
   file formats, exception handling). List each one.
3. **Non-blocking gaps:** count only, plus where they are recorded.
4. **Assumptions** that stage 4 will rely on.

Then ask, giving these options:
- **Proceed:** go to tech selection with the current requirements and
  the listed assumptions.
- **Revise:** the user answers clarifications or corrects requirements.
  Update the stage 2/3 outputs, then show the gate again.
- **Stop:** save state and end. The pipeline can be resumed later.

If blocking gaps exist, say plainly that a decision made now may change once
they are answered. Record the user's answer and the time in
`pipeline-state.md`.

## Stages 4-6

Run them in order, passing the saved outputs forward. Rules:
- Stage 5 must use the tech chosen in stage 4. If the user overrides that
  choice, record the override and the reason.
- Stage 6 must trace every test to a requirement ID from stage 2. Flag
  requirements with no test.
- If any stage reveals a new blocking gap, pause, tell the user, and ask
  whether to go back to stage 3.

## Stage 7: AE workflow build (Process Studio only)

Once stage 6 is done:
1. If stage 4 didn't choose Process Studio, skip stage 7, mark it
   `n/a` in the state file, and go to Finish.
2. Otherwise ask: "Ready to build the Process Studio workflow?" Start only
   on a yes. Otherwise save state and finish.
3. **7a** `ae-automation-designer`: plan the workflow from the SDD.
   Every requirement in scope for Process Studio must map to a step or a
   sub-workflow.
4. **7b** `ae-workflow-gen`: generate the files from the 7a plan. Save them in
   `07-workflow/`.
5. **7c** `ae-code-reviewer`: review the generated files. If it reports
   Critical or High findings, fix them with `ae-workflow-gen` and run the
   review again, up to 2 rounds. Then list any findings still open.
6. Check that each stage 6 test case has a matching path in the workflow,
   and flag tests that don't.

## pipeline-state.md format

```markdown
# Pipeline: <project-name>
| Stage | Status | Output file | Notes |
|-------|--------|-------------|-------|
| 0 Intake | done | — | BRD v1.2, flowchart; no samples |
| 1 Prior work | skipped | — | user declined |
| 2 Requirements | done | 02-requirements.json | 48 reqs |
| 3 Gap analysis | done | 03-gap-register.md | 3 blocking |
| GATE | approved 2026-11-20 14:05 | — | proceed with assumptions A1-A3 |
| 4 Tech selection | pending | | |
| 5 Solution design | pending | | |
| 6 Test design | pending | | |
| 7a AE design | pending | | |
| 7b AE workflow gen | pending | | |
| 7c AE code review | pending | | |

## Assumptions
## Decisions and overrides
```

## Resuming

If `pipeline-state.md` already exists, read it, tell the user where things
stand, and continue from the first stage not marked done. If the gate
isn't marked approved, show it again before stage 4.

## If a skill is unavailable

If a stage's skill isn't enabled, tell the user which one is missing and
ask them to choose: enable it, skip the stage (and record that), or have you
do the stage without the skill, clearly labelled as such. Never skip a stage
silently.

## Finish

When the last stage that applies is done, list every output file, the open
gaps and any open review findings. If the chosen tech isn't Process Studio,
suggest the build skill that fits: `playwright-portal-developer`
(Playwright) or `brd-flowchart-to-production-ready-drools-rule-engine`
(rules). Don't start that build unless the user asks.

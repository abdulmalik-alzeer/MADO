# Agent Instructions

You are an AI coding agent working on **[PROJECT_NAME]**. You operate under the MADO methodology — you follow structured documentation rather than relying on conversation memory.

## Session Workflow

At the start of every session:

1. **Read this file** to understand your role and rules.
2. **Read `.mado/STATUS.json`** to find the current step.
3. **Read the step file** referenced in `current_step.file`.
4. **Execute the work** described in the step file.
5. **Verify your work** against the step's acceptance criteria.
6. **Update `.mado/STATUS.json`** — mark the step as `completed`, advance `current_step` to the next `pending` step, recalculate `stats` from step statuses.
7. **Write a handoff note** (optional but recommended) at `.mado/handoffs/step-{phase}-{step}.md` if you made decisions, encountered issues, or have context the next session should know. Use the [handoff template](HANDOFF_TEMPLATE.md).
8. **Commit your changes** with a clear message referencing the step (e.g., "Complete step 1.2: User model and migration").

## Rules

- **One step per session.** Complete exactly one step, then stop. Do not continue to the next step.
- **Follow the step file.** Do not add features, refactor code, or make changes not described in the current step.
- **Do not assume.** If the step file is unclear or missing information, flag it rather than guessing.
- **Validate before completing.** Run tests and verify acceptance criteria before marking a step as done.
- **Update STATUS.json accurately.** This file must reflect reality. Do not mark a step `completed` unless it passes all acceptance criteria.
- **Recalculate stats, don't increment.** When updating `stats` in STATUS.json, count `completed` steps from the phases array. Do not manually increment — always derive the numbers from actual step statuses.
- **Do not modify previous steps' code** unless the current step explicitly instructs you to.

## When Things Go Wrong

- **Acceptance criteria fail.** Mark the step as `failed` in STATUS.json. Document what failed and why in a handoff note at `.mado/handoffs/`. Do not proceed to the next step.
- **A dependency is missing or broken.** Mark the step as `blocked` in STATUS.json. Note which dependency is missing and what step should have provided it. Stop and flag it to the user.
- **The step file is ambiguous or incomplete.** Do not guess. Stop and ask the user for clarification before writing any code.
- **The step is too large to complete in one session.** Stop and flag it to the user. The step needs to be split into smaller sub-steps before continuing.

## Project Context

- **Orchestration:** `.mado/ORCHESTRATION.md` — project overview, phases, global rules
- **Status:** `.mado/STATUS.json` — current state and progress
- **Steps:** `.mado/steps/` — detailed instructions for each step

## Tech Stack

[DESCRIBE YOUR TECH STACK HERE — language, framework, database, key dependencies]

## Project Conventions

[ADD ANY PROJECT-SPECIFIC CONVENTIONS — coding style, naming patterns, architectural decisions]
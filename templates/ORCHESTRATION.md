# Project Orchestration

## Overview

**Project:** [PROJECT_NAME]  
**Description:** [What this project does and who it's for, in 1-2 sentences]  
**Tech Stack:** [Language, framework, database, key dependencies]  
**Started:** [Date]

## Goals

What does success look like when this project is complete?

1. [GOAL_1 — concrete, measurable outcome]
2. [GOAL_2]
3. [GOAL_3]

## Phases

### Phase 1: [PHASE_NAME]

**Purpose:** [What this phase accomplishes]  
**Steps:** [N] steps — see `steps/step-01-*.md`

### Phase 2: [PHASE_NAME]

**Purpose:** [What this phase accomplishes]  
**Steps:** [N] steps — see `steps/step-02-*.md`

[Add more phases as needed]

## Global Rules

Rules that apply across every step. The agent must follow these throughout the entire project.

- [RULE — e.g., "All database tables use UUID primary keys"]
- [RULE — e.g., "Every public method must have a docblock"]
- [RULE — e.g., "Follow existing code patterns; do not introduce new architectural patterns without documenting the decision"]

## Phase Dependencies

Note critical ordering constraints between phases:

- Phase 2 requires Phase 1 complete (database must exist before business logic)
- Phase 4 can begin after Phase 3 Step 2 (API endpoints must exist for UI)

## Additional Context

[Any domain knowledge, external system details, stakeholder constraints, or design decisions that the agent needs to know across all steps. If this section grows large, consider moving it to a separate file in `.mado/context/`.]
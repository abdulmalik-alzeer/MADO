# Status Tracking

STATUS.json is the single source of truth for project state. It tells the agent (and you) what's been completed, what's in progress, and what's next.

## Why a JSON File

JSON is machine-readable. The agent can parse it programmatically, update it reliably, and you can inspect it at a glance. Markdown status tables are human-readable but fragile — agents frequently corrupt them during updates. JSON eliminates that problem.

## Structure

```json
{
  "project": {
    "name": "Your Project Name",
    "started_at": "2025-01-15T10:00:00Z",
    "last_updated": "2025-01-15T14:30:00Z"
  },
  "current_step": {
    "phase": 1,
    "step": 1,
    "file": ".mado/steps/step-01-01.md",
    "status": "pending"
  },
  "phases": [
    {
      "number": 1,
      "name": "Phase Name",
      "status": "in_progress",
      "steps": [
        {
          "number": 1,
          "name": "Step description",
          "file": ".mado/steps/step-01-01.md",
          "status": "pending"
        }
      ]
    }
  ],
  "stats": {
    "total_steps": 0,
    "completed_steps": 0,
    "completion_percentage": 0.0
  }
}
```

## Status Values

Steps and phases use the same status values:

| Status        | Meaning                                |
|---------------|----------------------------------------|
| `pending`     | Not yet started                        |
| `in_progress` | Currently being worked on              |
| `completed`   | Finished and validated                 |
| `failed`      | Attempted but did not pass validation  |
| `blocked`     | Cannot proceed due to unmet dependency |

## How the Agent Uses It

At the start of every session, the agent:

1. Reads `STATUS.json`
2. Looks at `current_step` to find the active task
3. Reads the step file referenced in `current_step.file`
4. Executes the work
5. Updates `STATUS.json`: marks the step `completed`, advances `current_step` to the next `pending` step, recalculates `stats` by counting step statuses from the phases array (never manually increment — always derive from actual data), and updates `last_updated`

## How You Use It

STATUS.json gives you a dashboard of project progress without opening the agent. At any point you can check:

- How far along the project is (`stats.completion_percentage`)
- What was completed last (`find the latest completed step`)
- What's next (`current_step`)
- Whether anything failed or is blocked

If you're managing multiple MADO projects, STATUS.json is the file you check first.

## Recovery

If a session crashes mid-step, STATUS.json tells you exactly where things stand. If `current_step.status` is `in_progress`, the step was interrupted. Revert any partial code changes and restart that step in a new session.

If the agent marked a step `completed` but the output is wrong, set it back to `pending` or `failed`, revert the code, and rerun.
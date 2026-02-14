# Directory Structure

MADO files live in a `.mado/` directory at the root of your project. The `AGENT.md` file sits at the project root so the agent finds it immediately.

## Standard Layout

```
your-project/
├── AGENT.md                    ← Agent reads this first every session
├── .mado/
│   ├── ORCHESTRATION.md        ← Project overview, phases, global rules
│   ├── STATUS.json             ← Progress tracking (source of truth)
│   ├── steps/                  ← One file per step
│   │   ├── step-01-01.md       ← Phase 1, Step 1
│   │   ├── step-01-02.md       ← Phase 1, Step 2
│   │   ├── step-02-01.md       ← Phase 2, Step 1
│   │   └── ...
│   └── handoffs/               ← Optional session handoff notes
│       └── ...
├── src/                        ← Your project source code
└── ...
```

## File Naming

Step files follow the pattern `step-{phase}-{step}.md` where both numbers are zero-padded to two digits. This keeps them sorted naturally in file listings.

Examples: `step-01-01.md`, `step-01-02.md`, `step-02-01.md`, `step-03-12.md`

## What Goes Where

**`AGENT.md` at project root.** This is the entry point. When you tell the agent "read AGENT.md and begin," it needs to find this file immediately. It contains the agent's role, rules, and pointers to the `.mado/` directory.

**`ORCHESTRATION.md` in `.mado/`.** The high-level project definition. Phases, goals, tech stack, global rules. The agent references this for big-picture context but doesn't modify it during execution.

**`STATUS.json` in `.mado/`.** Updated by the agent after every step. This is the only file the agent modifies within `.mado/` during normal execution.

**Step files in `.mado/steps/`.** One file per step. The agent reads the current step file but doesn't modify it — step files are instructions, not logs.

**Handoffs in `.mado/handoffs/` (optional).** If you want to capture notes between sessions — things the agent flagged, decisions made, issues encountered — handoff files provide a place for that. These are optional and primarily useful for your own reference.

## Customization

This structure is a starting point. Adapt it to your needs. Some teams add:

- `.mado/decisions/` for architectural decision records
- `.mado/context/` for reference documentation the agent needs (API specs, design docs, etc.)
- `.mado/validations/` for test results and validation logs

The only files that are structurally required for MADO to function are `AGENT.md`, `ORCHESTRATION.md`, `STATUS.json`, and the step files. Everything else is optional.
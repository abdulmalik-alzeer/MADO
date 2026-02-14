# MADO — Multi Agent Documentation Orchestrator

AI coding agents lose context, forget instructions, and produce inconsistent code on complex projects. MADO is a structured workflow that replaces conversation memory with persistent documentation — so your agent always knows where it is, what to do next, and what's already been done.

## The Problem

AI coding agents work well for small tasks. Ask one to build a utility function or scaffold a model, and it delivers. But ask it to build a multi-module system across dozens of sessions, and things fall apart:

**Context evaporates.** The agent forgets what it built two sessions ago. It creates duplicate code, contradicts earlier decisions, or overwrites working implementations.

**Token limits degrade quality.** Long conversations push against context windows. The agent starts hallucinating, ignoring instructions, or producing shallow output as earlier context gets pushed out.

**Recovery is impossible.** Session crashes, network failures, or accidental closures mean starting over. There's no checkpoint, no save state, no way to pick up where you left off.

**Scope drifts silently.** Without a persistent record of what's been completed and what's next, the agent loses track of the big picture. It solves the wrong problems or gold-plates features that don't matter yet.

These aren't edge cases. They're the default experience for anyone using AI agents on projects with more than a handful of features.

## How MADO Works

MADO shifts coordination from conversation memory to files on disk. The core loop is simple:

```
Define project → Decompose into steps → Agent reads step file → Agent executes
→ Agent updates STATUS.json → Clear session → Next step
```

Each step is self-contained. The agent doesn't need to remember previous sessions because everything it needs — context, instructions, acceptance criteria — lives in documentation it reads fresh at the start of every session.

**Session isolation** is the key insight. Instead of one long, degrading conversation, you run many short, focused sessions. Each session starts clean, reads the current step file, does the work, and updates the tracking file. If a session fails, you restart it. Nothing is lost.

### The Core Files

| File               | What It Does                                                                                  |
|--------------------|-----------------------------------------------------------------------------------------------|
| `AGENT.md`         | Instructions the AI agent reads at the start of every session — its role, rules, and workflow |
| `ORCHESTRATION.md` | Project overview: goals, phases, tech stack, and global rules                                 |
| `STATUS.json`      | Machine-readable state: current step, progress, completion stats                              |
| `steps/*.md`       | One file per step with objectives, tasks, and acceptance criteria                             |

That's it. Four types of files. Everything else is optional.

## Quick Start

**1. Copy the templates into your project:**

```
your-project/
├── AGENT.md                  ← agent reads this every session
├── .mado/
│   ├── ORCHESTRATION.md      ← project overview and phases
│   ├── STATUS.json           ← progress tracker
│   └── steps/
│       ├── step-01-01.md     ← phase 1, step 1
│       ├── step-01-02.md     ← phase 1, step 2
│       └── ...
├── src/
└── ...
```

**2. Fill in ORCHESTRATION.md** with your project name, tech stack, goals, and phase breakdown.

**3. Decompose your project into steps** using the [step decomposition guide](docs/step-decomposition.md). Each step should be completable in a single agent session. Create a step file for each using the [step template](templates/STEP_TEMPLATE.md).

**4. Initialize STATUS.json** with your project metadata and step list.

**5. Point your AI agent at AGENT.md** and tell it to begin. After each step, the agent updates STATUS.json, you clear the session, and start fresh for the next step.

## When to Use MADO

MADO adds value when your project is **too large for a single session**. If you're building something with multiple features, modules, or phases — and you'll need many agent sessions to complete it — MADO provides the structure to keep everything consistent and recoverable.

For small, single-session tasks (a utility function, a bug fix, a simple script), MADO is overkill. Just prompt the agent directly.

## Documentation

| Document                                           | Description                                       |
|----------------------------------------------------|---------------------------------------------------|
| [Methodology](docs/methodology.md)                 | The full MADO workflow explained step by step     |
| [Step Decomposition](docs/step-decomposition.md)   | How to break projects into agent-executable steps |
| [Status Tracking](docs/status-tracking.md)         | How STATUS.json works and how to use it           |
| [Directory Structure](docs/directory-structure.md) | File organization reference                       |

## Templates

All templates are in the [`templates/`](templates/) directory, ready to copy into your project:

- [`AGENT.md`](templates/AGENT.md) — Agent session instructions
- [`ORCHESTRATION.md`](templates/ORCHESTRATION.md) — Project orchestration document
- [`STATUS.json`](templates/STATUS.json) — Progress tracking file
- [`STEP_TEMPLATE.md`](templates/STEP_TEMPLATE.md) — Step file template
- [`HANDOFF_TEMPLATE.md`](templates/HANDOFF_TEMPLATE.md) — Session handoff notes (optional)

## Scope and Limitations

MADO was developed and tested by a single developer using **Claude Code** on **Laravel/Filament** projects. The core concepts — persistent documentation, session isolation, step decomposition — are designed to be agent-agnostic and stack-agnostic, but this has not yet been validated beyond that environment.

What this means in practice:

- The workflow pattern should transfer to any AI agent that can read files (Cursor, Copilot, Aider, etc.) — but "should" is not "proven"
- The templates may need adaptation for your tools and stack
- Your mileage will vary depending on project complexity, agent capability, and how well you decompose your steps

This is an honest sharing of a workflow that produced strong results in a specific context. It is not a universal guarantee. If you try MADO in a different environment, your experience — good or bad — would be a genuinely valuable contribution. See [Contributing](#contributing).

## Contributing

MADO is early-stage and community validation is the most important thing that can happen next. The most valuable contributions right now:

**Testing reports.** Tried MADO with a different agent or stack? Open an issue describing what worked, what didn't, and what you had to adapt. Even a paragraph is useful.

**Template improvements.** Found that a template field is confusing, missing, or unnecessary? Suggest a change.

**Step decomposition examples.** If you decomposed a project using MADO and are willing to share the step structure (even without proprietary details), it would help others understand how to apply the methodology.

## License

MIT — do whatever you want with it. See [LICENSE](LICENSE).
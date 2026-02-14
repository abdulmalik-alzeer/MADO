# MADO Methodology

This document explains the complete MADO workflow from project definition through completion.

## Core Principle

Traditional AI coding workflows rely on conversation memory. You tell the agent what to build, it works on it, and the conversation grows longer as the project evolves. This works for small tasks but breaks on complex projects — the context window fills up, output quality degrades, and if the session dies, you lose everything.

MADO replaces conversation memory with persistent documentation. Every piece of context the agent needs lives in files, not in chat history. Sessions can be cleared without losing progress. Any agent session can pick up where the last one left off. Failures are recoverable by checking the last known state in STATUS.json.

## The Workflow

### Phase 1: Project Definition

Before any agent writes a line of code, you define the project through documentation.

**Write ORCHESTRATION.md.** Define the project name, tech stack, high-level goals, and phases. This is the table of contents for the entire build. It gives the agent (and you) a shared understanding of what's being built and why.

**Decompose into steps.** Break each phase into granular steps. Each step should be small enough that an AI agent can complete it in a single session without context degradation. This is the hardest and most important part of MADO. See [step-decomposition.md](step-decomposition.md) for detailed guidance.

**Initialize STATUS.json.** Set up the tracking file with your project metadata, phases, and step list. This becomes the single source of truth for project state — what's done, what's in progress, what's next.

**Configure AGENT.md.** Write the instructions your AI agent will read at the start of every session. This tells the agent its role, its rules, and where to find its current task. Think of it as onboarding documentation for every fresh session.

### Phase 2: Execution

Once documentation is ready, execution follows a repeating cycle:

```
┌─────────────────────────────┐
│  1. Start new session       │
│     Agent reads AGENT.md    │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  2. Agent checks STATUS.json│
│     Finds the current step  │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  3. Agent reads step file   │
│     Gets full instructions  │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  4. Agent executes the work │
│     Creates/modifies code   │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  5. Agent self-validates    │
│     Checks acceptance       │
│     criteria                │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  6. Agent updates           │
│     STATUS.json             │
│     Marks step completed    │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  7. Clear session           │
│     Start fresh for next    │
│     step                    │
└─────────────────────────────┘
```

This cycle repeats until all steps are complete. Each session is isolated — the agent starts fresh every time and relies entirely on the documentation to understand context.

### Phase 3: Recovery

When something goes wrong — and it will — MADO's file-based approach makes recovery straightforward.

**Session crash or network failure.** Check STATUS.json. If the current step is still marked `in_progress`, restart that step in a new session. The agent reads the step file, sees what needs to be done, and picks up the work.

**Agent produced incorrect output.** Revert the code changes from the failed step (git makes this easy), keep STATUS.json as-is, and rerun the step. You might improve the step file's instructions or acceptance criteria before retrying.

**Step was too large.** If an agent consistently fails a step, the step is probably too broad. Split it into smaller sub-steps. This is a normal part of the process, not a failure of the methodology.

## Key Principles

**One step per session.** Don't let the agent continue to the next step in the same session. Clear the session and start fresh. This prevents context accumulation and keeps quality high.

**Smaller steps are better.** When in doubt, make the step smaller. A step that's too granular wastes a little setup time. A step that's too broad produces unreliable output. Err on the side of granularity.

**Documents are the source of truth.** The agent should never need to "remember" something from a previous session. If information matters, it should be in a file.

**Validate before marking complete.** After each step, verify the output meets acceptance criteria before updating STATUS.json. This prevents cascading errors where later steps build on broken foundations.

**Clear aggressively.** When in doubt, clear the session. A fresh start with good documentation beats a long conversation with accumulated confusion.
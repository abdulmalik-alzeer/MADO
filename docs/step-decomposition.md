# Step Decomposition

Step decomposition is the most important skill in MADO. The quality of your steps directly determines the quality of the AI agent's output. This guide explains how to break a project into steps that an agent can execute reliably.

## The Rule

A step should be completable in a single agent session without exceeding the agent's effective context window.

If a step requires the agent to juggle too many files, dependencies, or requirements at once, it's too big. If a step is so trivial it doesn't produce a meaningful change, it's too small. The sweet spot: a step that produces a testable, committable unit of work in one focused session.

## Start with Phases

Phases are high-level groupings of related work. A typical project might have:

- **Phase 1:** Project setup and configuration
- **Phase 2:** Core data models and database
- **Phase 3:** Business logic and services
- **Phase 4:** User interface
- **Phase 5:** Authentication and authorization
- **Phase 6:** Integrations and APIs
- **Phase 7:** Testing, polish, and deployment

Your phases will vary by project. The purpose is organizational clarity — grouping related steps so the project has a readable narrative arc.

## Break Phases into Steps

Each phase contains multiple steps. Granularity matters enormously here.

**Too broad — agent will produce unreliable output:**

> Step 1: Build the authentication system

This is an entire feature with dozens of decisions. The agent will lose focus, make contradictory choices, and likely produce code that doesn't hold together.

**Too narrow — overhead exceeds value:**

> Step 1: Create the users table migration  
> Step 2: Add the email column  
> Step 3: Add the password column

These could all be a single step. Splitting them creates unnecessary session overhead.

**Right-sized — one coherent unit of work:**

> Step 1: Create user model, migration, and factory with core fields (email, password, name, role)  
> Step 2: Implement registration endpoint with input validation and email verification  
> Step 3: Implement login/logout with session management  
> Step 4: Add role-based middleware and route protection

Each step produces a testable result and has clear boundaries.

## Writing a Step File

Every step file should contain five things:

**Objective.** What this step accomplishes in 1-2 sentences. This grounds the agent's understanding of why it's doing this work.

**Dependencies.** What must exist before this step can run. Reference specific step numbers and the files or features they produced. Don't make the agent guess.

**Tasks.** The specific things the agent should do, in execution order. Be explicit. "Create a model" is vague. "Create a User model at `app/Models/User.php` with `email`, `password`, and `name` fields, and a `hasMany` relationship to `Post`" is actionable.

**Acceptance criteria.** How to verify the step is complete. These should be concrete and testable: "Migration runs without errors", "POST /register returns 201 with valid input", "All existing tests still pass". The agent uses these to self-validate.

**Files to create or modify.** Explicitly list which files the agent will touch. This sets scope boundaries and prevents the agent from wandering into unrelated code.

See the [step template](../templates/STEP_TEMPLATE.md) for the standard format.

## Dependency Management

Steps should be ordered so that dependencies are already completed. If Step 3.2 requires the database schema from Step 2.1, that ordering should be explicit.

In each step file, reference dependencies clearly:

> **Depends on:** Step 2.1 — User model and migration must exist at `app/Models/User.php`

This tells the agent exactly what it can rely on.

## Common Mistakes

**Assuming the agent remembers.** Every step should contain all the context the agent needs for that step. Don't write "use the same approach as Step 3." Either describe the approach or reference the specific file where it's implemented.

**Mixing concerns.** A step that says "build the API endpoint AND the frontend form AND the validation logic" is three steps compressed into one. Separate them. The agent will handle each better in isolation.

**Vague acceptance criteria.** "It should work" is not testable. "The `/api/users` endpoint returns a 200 response with a JSON array of user objects" is.

**Skipping the decomposition.** It's tempting to jump straight to prompting the agent. Resist. Thirty minutes spent decomposing a project into well-defined steps will save hours of debugging bad output from sessions that tried to do too much.

## Scale Reference

From experience, project scale roughly maps to step count like this:

| Project Size | Phases | Steps  | Example                                     |
|--------------|--------|--------|---------------------------------------------|
| Small        | 2-3    | 5-15   | Simple API, landing page with CMS           |
| Medium       | 4-5    | 15-30  | SaaS MVP, admin dashboard                   |
| Large        | 6-8    | 30-50+ | Multi-tenant platform, complex business app |

These are rough guides, not rules. The right number of steps is however many it takes to make each step small enough for reliable agent execution.
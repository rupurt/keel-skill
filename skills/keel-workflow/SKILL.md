---
name: keel-workflow
description: Keel SDLC workflow for repositories that use `just keel`. Use when the task is to implement a story, decompose an epic or voyage, author PRD/SRS/SDD planning artifacts, record acceptance-criterion evidence, run Keel lifecycle transitions, or research ambiguous work through bearings.
---

# Keel Workflow

Use this skill to apply the Keel operating model without re-deriving the board rules.

## Quick Start

1. Classify the request as implementation, planning, or research work.
2. Read [`references/workflow-reference.md`](references/workflow-reference.md) before creating artifacts or running lifecycle transitions.
3. Use `just ...` for repository build, quality, and test workflows.
4. Use `just keel ...` for board discovery, lifecycle, and verification workflows.

## Choose The Mode

### Implementer

Use this path when the user is coding an existing story or wants the next implementation item.

- Start with `just keel flow`.
- Claim work with `just keel next --agent` unless the user already specified the target.
- Verify traceability and proof strategy before coding.
- Follow TDD, record evidence, reflect, commit once, and submit.

### Architect

Use this path when the user is planning new work or refining existing planning artifacts.

- Use `just keel flow` or `just keel status` to find the next gap.
- Create the epic or voyage through the CLI.
- Fill PRD, SRS, and SDD content before decomposing stories.
- Align verification with `just keel config show`, `just keel verify detect`, and `just keel verify recommend`.
- Publish a planning summary in chat, commit once, and seal the voyage.

### Explorer

Use this path when the work is ambiguous enough to require deliberate exploration.

- Create a bearing through the CLI.
- Keep the required `README.md`, `BRIEF.md`, `SURVEY.md`, and `ASSESSMENT.md` structure.
- Use `just keel play <id>` when exploratory masks are useful.
- Commit once and graduate the bearing only when the recommendation is conclusive.

## Non-Negotiables

- Use CLI transitions only. Never move `.keel` files manually.
- Create exactly one atomic Conventional Commit per story, planning unit, or bearing.
- Run `just keel doctor`, `just quality`, and `just test` before finalizing unless blocked.
- Prefer hard cutover: replace old paths in the same slice unless the story explicitly calls for migration or compatibility work.
- Keep requirement-to-story and acceptance-criterion-to-evidence links explicit.

## Reference

- [`references/workflow-reference.md`](references/workflow-reference.md) contains the exact workflow steps, validation gates, state transitions, and command catalog.

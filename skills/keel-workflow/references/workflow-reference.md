# Keel Workflow Reference

Use this reference after the `keel-workflow` skill triggers.

## Select The Mode

- **Implementer**: Use when the task is to code a story, complete acceptance criteria, or move existing work toward submission.
- **Architect**: Use when the task is to define or revise epics, voyages, stories, PRDs, SRS documents, SDD documents, or verification plans.
- **Explorer**: Use when the path forward is unclear and the work needs deliberate research through a bearing.

## Implementer Workflow

1. Inspect board health with `just keel flow`.
2. Claim work with `just keel next --agent` unless the user already named the story.
3. Verify story coherence before coding:
   - Acceptance criteria map to source requirements such as `[SRS-XX/AC-YY]`.
   - Every criterion has a proof strategy.
   - Ambiguity sends you back to planning artifacts before coding.
4. Execute with TDD:
   - Write a failing test.
   - Implement the smallest passing change.
   - Refactor inside the same slice.
5. Record proof for each acceptance criterion:
   - `just keel story record <id> --ac <num> --msg "<proof>"`
6. Reflect with `just keel story reflect <id>`.
7. Run the hygiene gate:
   - `just keel doctor`
   - `just quality`
   - `just test`
8. Create exactly one atomic Conventional Commit for the story.
9. Submit with `just keel story submit <id>`.

## Architect Workflow

1. Find the next planning gap with `just keel flow` or `just keel status`.
2. Create the planning unit through the CLI:
   - `just keel epic new "<Title>" --goal "<Outcome>"`
   - `just keel voyage new "<Title>" --epic <epic-id> --goal "<Specific outcome>"`
3. Fill the epic PRD before decomposition. Required sections:
   - `Problem Statement`
   - `Goals & Objectives`
   - `Users`
   - `Scope`
   - `Requirements`
   - `Verification Strategy`
   - `Assumptions`
   - `Open Questions & Risks`
   - `Success Criteria`
4. Fill `SRS.md` with atomic, uniquely identified requirements such as `SRS-01`.
5. Fill `SDD.md` with the architectural approach and the component changes needed for objective verification.
6. Create stories with `just keel story new ...` and link acceptance criteria to requirements with `[SRS-XX/AC-YY]`.
7. Align verification with:
   - `just keel config show`
   - `just keel verify detect`
   - `just keel verify recommend`
8. Run a downstream coherence review:
   - Every requirement maps to at least one story.
   - Every acceptance criterion has a verification path.
   - Verification commands align with the recommended techniques unless explicitly justified.
9. Publish a planning summary in chat with scope, coverage, verification, risks, and the next command.
10. Create exactly one atomic Conventional Commit for the planning unit.
11. Seal planning with `just keel voyage plan <id>`.

## Explorer Workflow

1. Create a bearing with `just keel bearing new "<Name>"`.
2. Keep the mandatory structure:
   - `README.md`
   - `BRIEF.md`
   - `SURVEY.md`
   - `ASSESSMENT.md`
3. Use `just keel play <id>` for discovery through different masks or perspectives.
4. Fill `BRIEF.md` with:
   - `Hypothesis`
   - `Problem Space`
   - `Success Criteria`
   - `Open Questions`
5. Record research and technical constraints in `SURVEY.md`.
6. Seal the survey with `just keel bearing survey <id>`.
7. Record the recommendation in `ASSESSMENT.md`.
8. Seal the assessment with `just keel bearing assess <id>`.
9. Create exactly one atomic Conventional Commit for the bearing package.
10. Graduate conclusive research with `just keel bearing lay <id>`.

## Hard Rules

- Use `just ...` for repository workflows and `just keel ...` for board workflows.
- Use CLI transitions only. Do not move `.keel` files manually.
- Create one atomic Conventional Commit per story, planning unit, or bearing.
- Run `just keel doctor`, `just quality`, and `just test` before finalizing unless blocked.
- Prefer hard cutover. Do not add compatibility bridges unless the story explicitly requires them.

## Core Commands

### Repo Commands

- `just`
- `just setup`
- `just build`
- `just build-debug`
- `just build-release`
- `just run`
- `just test`
- `just quality`
- `just coverage`
- `just pre-commit`

### Board Commands

- `just keel flow`
- `just keel status`
- `just keel next --agent`
- `just keel doctor`
- `just keel config show`
- `just keel verify detect`
- `just keel verify recommend`
- `just keel epic new`
- `just keel voyage new`
- `just keel voyage plan`
- `just keel story new`
- `just keel story start`
- `just keel story record`
- `just keel story reflect`
- `just keel story submit`
- `just keel bearing new`
- `just keel play`
- `just keel bearing survey`
- `just keel bearing assess`
- `just keel bearing lay`

## State Changes

- Story start: `just keel story start <id>`
- Story reflect: `just keel story reflect <id>`
- Story submit: `just keel story submit <id>`
- Story reject: `just keel story reject <id> "reason"`
- Story accept: `just keel story accept <id>`
- Story ice: `just keel story ice <id>`
- Story thaw: `just keel story thaw <id>`
- Voyage done: `just keel voyage done <id>`

## Foundational Documents

Interpret constraints in this order:

1. `.keel/adrs/`
2. `CONSTITUTION.md`
3. `ARCHITECTURE.md`
4. Planning artifacts

# AGENTS.md

Shared guidance for AI coding harnesses working with a Keel-managed repository.
Import this file from harness-specific context files when supported.

## Execution Workflow (Implementer)

1. **Pull Context**: Read current board health and identify bottlenecks with `just keel flow`.
2. **Claim Work**: Pull the highest-priority implementation item with `just keel next --agent`. Use `--parallel` to identify safe concurrent tasks.
3. **Check Story Coherence Before Coding**: Confirm acceptance criteria are traceable and verifiable:
   - Acceptance criteria are linked to source requirements such as `[SRS-XX/AC-YY]`.
   - Evidence strategy is clear for each criterion: automated test, CLI proof, or manual proof.
   - If requirements are ambiguous, loop back to planning artifacts before implementation.
4. **Execute With TDD**:
   - Write a failing test first.
   - Implement only enough to pass.
   - Refactor within the same change slice.
5. **Record Evidence**: Capture proof for each acceptance criterion:
   - `just keel story record <ID> --ac <NUM> --msg "Description of the proof"`
   - For manual proof, use `--msg` or editor integration.
6. **Reflect**: Run `just keel story reflect <ID>` and capture what was learned during implementation.
7. **Commit**: Create exactly one atomic Conventional Commit for the story. Do not batch multiple stories into one commit.
8. **Submit**: Move the story to the human review queue with `just keel story submit <ID>`. This triggers automated verification and generates the verification manifest.

## Planning Workflow (Architect)

1. **Identify Gaps**: Use `just keel flow` or `just keel status` to find epics that need tactical decomposition.
2. **Scaffold the Planning Unit**:
   - New strategic work: `just keel epic new "<Title>" --goal "<Outcome>"`
   - Tactical decomposition: `just keel voyage new "<Title>" --epic <epic-id> --goal "<Specific outcome>"`
3. **Author the Epic PRD Immediately After Creation**: Before decomposition, fill every required section in `epics/<epic-id>/PRD.md`:
   - `## Problem Statement`
   - `## Goals & Objectives`
   - `## Users`
   - `## Scope` with `### In Scope` and `### Out of Scope`
   - `## Requirements` with `FUNCTIONAL_REQUIREMENTS` and `NON_FUNCTIONAL_REQUIREMENTS`
   - `## Verification Strategy`
   - `## Assumptions`
   - `## Open Questions & Risks`
   - `## Success Criteria` with `SUCCESS_CRITERIA`
   - Optional: `PRESS_RELEASE.md` only for large user-facing shifts
4. **Define Requirements in SRS**: Fill `SRS.md` with atomic, uniquely identified requirements such as `SRS-01` that map cleanly to stories and evidence.
5. **Detail the Design in SDD**: Fill `SDD.md` with the architectural approach and component changes needed for objective verification.
6. **Decompose Stories**:
   - `just keel story new "<Title>" --epic <epic-id> --voyage <voyage-id>`
   - Link story acceptance criteria to SRS requirements with `[SRS-XX/AC-YY]`.
7. **Align Verification Techniques From Config**:
   - `just keel config show`
   - `just keel verify detect`
   - `just keel verify recommend`
   - If required techniques are missing or disabled, update `keel.toml` before continuing.
8. **Run a Downstream Coherence Review**:
   - Every SRS requirement has at least one linked story acceptance criterion.
   - Every acceptance criterion has a clear verification approach.
   - Verification commands align with `just keel verify recommend` unless explicitly justified.
   - CLI options and authored content are explicit enough for automation.
9. **Loop Upstream if Needed**: If decomposition exposes ambiguity, fix PRD, SRS, or SDD first, then re-check the stories.
10. **Publish a Planning Summary in Chat**: For every new epic or voyage, publish a terse planning summary in the harness response:
   - Objective and scope boundaries
   - Requirement-to-story coverage status
   - Verification strategy summary
   - Key risks and assumptions
   - Canonical next-step command
11. **Commit**: Create exactly one atomic Conventional Commit for the planning unit. Do not batch unrelated planning units.
12. **Seal Planning**: Promote the voyage with `just keel voyage plan <id>`. This validates requirement coverage and thaws stories into the backlog.

## Research Workflow (Explorer)

1. **Identify Fog**: Create a bearing when the path forward is ambiguous or requires exploration:
   - `just keel bearing new "<Name>"`
   - Always scaffold through the CLI.
2. **Respect Bearing Structure**: A bearing must contain:
   - `README.md`
   - `BRIEF.md`
   - `SURVEY.md`
   - `ASSESSMENT.md`
3. **Discovery**: Use `just keel play <id>` to explore the problem space through different masks or perspectives.
4. **Draft the Brief**: `BRIEF.md` must include:
   - `## Hypothesis`
   - `## Problem Space`
   - `## Success Criteria`
   - `## Open Questions`
5. **Survey Findings**: Document research, market context, and technical constraints in `SURVEY.md`.
6. **Seal Survey**: Transition with `just keel bearing survey <id>`.
7. **Assess Impact**: Document Proceed, Park, or Decline in `ASSESSMENT.md`.
8. **Seal Assessment**: Transition with `just keel bearing assess <id>`.
9. **Commit**: Create exactly one atomic Conventional Commit for the bearing research package.
10. **Graduate**: If the research is conclusive, lay the bearing into an epic with `just keel bearing lay <id>`.

## Global Hygiene Checklist

Apply these checks to every logical unit of work before finalizing:

1. **Doctor Check**: `just keel doctor` passes with zero warnings or errors.
2. **Quality Check**: `just quality` is clean.
3. **Verification**: `just test` passes.
4. **Atomic Commits**: Commit once per story, planning unit, or bearing using Conventional Commits:
   - `feat:`
   - `fix:`
   - `docs:`
   - `refactor:`
   - `test:`
   - `chore:`

## Compatibility Policy (Hard Cutover)

This repository uses a hard cutover policy by default.

1. **No Backward Compatibility by Default**: Do not add compatibility aliases, dual-write logic, soft deprecations, or fallback parsing unless a story explicitly requires it.
2. **Replace, Do Not Bridge**: Remove the old path in the same change slice when introducing a new canonical token, field, command behavior, or document contract.
3. **Fail Fast in Validation**: Treat legacy or unfilled scaffold patterns as hard failures when they violate the new contract.
4. **Single Canonical Path**: Keep one source of truth for rendering, parsing, and validation. Avoid parallel implementations added only for compatibility.
5. **Make Migration Explicit Work**: If artifacts need updates, handle them in a dedicated migration pass or story instead of embedding runtime compatibility logic.

## Foundational Documents

Interpret constraints in this order:

1. `.keel/adrs/`
2. `CONSTITUTION.md`
3. `ARCHITECTURE.md`
4. Planning artifacts

Use these documents as the canonical sources of truth:

- `README.md`
- `.keel/adrs/`
- `CONSTITUTION.md`
- `ARCHITECTURE.md`

## Project Overview

This layout describes the standard `keel` Rust crate and board structure.

| Path | Purpose |
|------|---------|
| `Cargo.toml` | Crate manifest with a single `[[bin]]` target |
| `src/` | Rust source organized by `cli`, `application`, `domain`, `infrastructure`, and `read_model` |
| `justfile` | Task runner recipes |
| `flake.nix` | Nix development environment |

### Board Directory (`.keel/`)

The `.keel/` directory is the runtime data directory that `keel` operates on in the managed project.

| Path | Contents |
|------|----------|
| `.keel/adrs/` | Architecture decision records |
| `.keel/epics/` | Epic planning artifacts |
| `.keel/epics/<epic-id>/voyages/` | Voyage planning artifacts such as `SRS.md` and `SDD.md` |
| `.keel/stories/` | Implementable work items |
| `.keel/README.md` | Auto-generated board state overview |

## Command Execution Model

Use one path per concern:

- `just ...` for repo build, quality, and test workflows
- `just keel ...` for board and lifecycle workflows

### Core `just` Commands

| Command | Purpose |
|---------|---------|
| `just` | List available recipes |
| `just setup` | Install helper tooling |
| `just build` | Alias to `just build-debug` |
| `just build-debug` | Build the debug artifact |
| `just build-release` | Build the release artifact |
| `just run` | Run the CLI |
| `just test` | Run the test suite |
| `just quality` | Run formatting and linting checks |
| `just coverage` | Produce `coverage/lcov.info` |
| `just pre-commit` | Run quality and tests |

### Core `just keel` Commands

| Category | Commands |
|----------|----------|
| Discovery | `just keel bearing new <name>` `just keel bearing survey <id>` `just keel bearing assess <id>` `just keel bearing list` |
| Planning | `just keel epic new <name> --goal <goal>` `just keel voyage new <name> --epic <epic-id> --goal <goal>` |
| Execution | `just keel story new "<title>" --epic <epic-id> --voyage <voyage-id>` |
| Board Ops | `just keel next --agent` `just keel next` `just keel status` `just keel flow` `just keel doctor` `just keel generate` `just keel config show` |
| Lifecycle | Use the story, voyage, epic, and bearing transitions below |

## Story and Milestone State Changes

Use CLI transitions only. Do not move `.keel` files manually.

| Action | Command |
|--------|---------|
| Start | `just keel story start <id>` |
| Reflect | `just keel story reflect <id>` |
| Submit | `just keel story submit <id>` |
| Reject | `just keel story reject <id> "reason"` |
| Accept | `just keel story accept <id>` |
| Ice | `just keel story ice <id>` |
| Thaw | `just keel story thaw <id>` |
| Voyage done | `just keel voyage done <id>` |

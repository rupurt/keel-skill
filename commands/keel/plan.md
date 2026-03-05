---
description: "Run the Keel architect workflow for epic or voyage planning"
argument-hint: "[goal, epic, or voyage]"
---

# Keel Plan

Use the `keel-workflow` skill.

Apply the Keel planning workflow to the current request.

Target: "$ARGUMENTS"

1. Use the provided target if one is given. Otherwise inspect the board with `just keel flow` or `just keel status` to find the next planning gap.
2. Scaffold the right planning unit with `just keel epic new ...` or `just keel voyage new ...`.
3. Author complete PRD, SRS, and SDD content before decomposing stories.
4. Link story acceptance criteria to requirements with `[SRS-XX/AC-YY]`.
5. Run `just keel config show`, `just keel verify detect`, and `just keel verify recommend` before sealing verification design.
6. Perform a downstream coherence review so every requirement maps to at least one story and every acceptance criterion has a proof path.
7. Publish a terse planning summary in chat with scope, coverage, verification, risks, and the next command.
8. Create exactly one atomic Conventional Commit for the planning unit.
9. Seal planning with `just keel voyage plan <id>` when the unit is ready.

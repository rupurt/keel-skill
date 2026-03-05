---
description: "Run the Keel implementer workflow for a story or coding task"
argument-hint: "[story-id or task]"
---

# Keel Implement

Use the `keel-workflow` skill.

Apply the Keel implementer workflow to the current task.

Target: "$ARGUMENTS"

1. If the arguments identify a story or task, use that target. Otherwise inspect the board with `just keel flow` and claim work with `just keel next --agent`.
2. Confirm every acceptance criterion is traceable to source requirements and has a proof strategy before coding.
3. Execute with TDD: failing test, minimal implementation, refactor in the same slice.
4. Record proof with `just keel story record <id> --ac <num> --msg "<proof>"` for each acceptance criterion.
5. Run `just keel story reflect <id>`.
6. Run `just keel doctor`, `just quality`, and `just test` unless blocked. If blocked, explain the blocker precisely.
7. Create exactly one atomic Conventional Commit for the story.
8. Submit with `just keel story submit <id>`.
9. Report the changes, the evidence captured, and any remaining blockers or risks.

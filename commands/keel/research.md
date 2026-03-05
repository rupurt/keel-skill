---
description: "Run the Keel explorer workflow for ambiguous or research-heavy work"
argument-hint: "[research topic]"
---

# Keel Research

Use the `keel-workflow` skill.

Apply the Keel research workflow to the current topic.

Target: "$ARGUMENTS"

1. Create or identify the relevant bearing with `just keel bearing new "<name>"`.
2. Keep the standard bearing structure: `README.md`, `BRIEF.md`, `SURVEY.md`, and `ASSESSMENT.md`.
3. Use `just keel play <id>` when discovery through different masks or perspectives is useful.
4. Fill `BRIEF.md`, `SURVEY.md`, and `ASSESSMENT.md` with concrete research findings and a recommendation.
5. Seal the survey with `just keel bearing survey <id>` and the assessment with `just keel bearing assess <id>`.
6. Create exactly one atomic Conventional Commit for the bearing package.
7. If the research is conclusive, graduate it with `just keel bearing lay <id>`.
8. Report the recommendation, supporting evidence, open questions, and the next command to run.

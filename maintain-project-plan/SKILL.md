---
name: maintain-project-plan
description: "Maintain a project-level, opt-in PLAN.md checklist workflow for personal software projects. Use whenever starting implementation, fixes, refactors, or other repository changes. Inspect the project-root PLAN.md for the persistent enable marker; when absent, ask whether to enable the workflow and do not write PLAN.md until the user agrees. Once enabled, use the same PLAN.md for all future development tasks and conversations in that project, recording requested outcomes before implementation and checking them off only after verification."
---

# Maintain Project Plan

Use the project-root `PLAN.md` as both the persistent project-level opt-in record and the living checklist. Do not require or modify `AGENTS.md` for this workflow. Enabling the workflow applies to all future development tasks and conversations in that project until the user disables it.

## Determine project state

1. Resolve the project root from the repository root; when no repository exists, use the active project directory.
2. Read `PLAN.md` when it exists.
3. Treat the workflow as enabled only when `PLAN.md` contains this exact marker:

```markdown
<!-- maintain-project-plan: enabled -->
```

4. When the marker is absent, ask the user whether to enable the `PLAN.md` workflow for this project.
   - Prefer a short yes/no user-input prompt when the surface supports it.
   - Do not create or edit `PLAN.md` before explicit approval.
   - If the user declines, continue the requested work without touching `PLAN.md`. Because no project-level state is written, a future development task may ask again.
5. Explicitly invoking this skill does not replace the per-project approval unless the user also clearly says to enable it.

## Enable the workflow

After approval:

- Create `PLAN.md` when absent, placing the marker on the first line.
- Add the marker to the first line of an existing `PLAN.md` while preserving all existing content and formatting.
- Do not add an `AGENTS.md` opt-in rule.
- Treat the marker as persistent approval for every later development task in the project; do not ask again while it remains present.

Use this minimal format unless the existing file establishes another checklist style:

```markdown
<!-- maintain-project-plan: enabled -->

- [ ] Requested outcome.
    - [ ] Important acceptance condition.
```

## Maintain the checklist

- Before implementation, add unchecked items for the user-visible outcomes and material acceptance conditions in the current request.
- Keep entries concise and behavior-focused. Do not copy a verbose implementation plan into the checklist.
- Preserve existing sections, indentation, wording, checked state, and unrelated tasks.
- Update an existing matching item instead of creating a duplicate.
- Add newly requested scope as unchecked items, including additions made later in the same task.
- Mark an item `[x]` only after its outcome is implemented and verified proportionally to risk.
- Mark parent items complete only when all required child items are complete.
- Leave incomplete or blocked work unchecked. Add a concise note only when it helps explain the remaining work.
- Never check items merely because code was written, a plan was proposed, or the user said to proceed.

## Finish the task

- Re-read `PLAN.md` after verification and make its checkbox state match the actual result.
- Mention the updated checklist briefly in the final handoff.
- Do not mark unrelated historical items complete without evidence from the current work.

## Disable or bypass

- If the user asks to disable the workflow for an enabled project, remove only the marker and preserve the checklist content.
- If the user asks to skip planning for one task, leave the marker unchanged but do not update `PLAN.md` for that task.
- Explicit user instructions and repository rules always override this workflow.

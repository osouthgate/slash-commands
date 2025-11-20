---
description: Finalize a completed task or plan with status, summary, and affected files
argument-hint: TASK_FILE=<path-to-task.md>
---

# Finalize Task / Plan (slash command)

Use this template whenever a task is completed and needs to be closed out & moved into `docs/tasks/finished/`.

Fields to fill:
- Status: short, date-stamped completion note.
- Summary of work delivered: bullet list of the key changes/decisions and validations (tests/builds).
- Affected files: paths (one per line) for traceability.
- Follow-up (optional): anything still pending or nice-to-have.
- Move instruction: rename the original task file to `docs/tasks/finished/<XX>-DONE-<name>.md`.

Suggested prompt body:
```
Status: Done — implemented on <YYYY-MM-DD>.
Summary of work delivered
- <bullet 1>
- <bullet 2>
- <tests/builds run>
- <notable decisions/constraints>
Affected files
- <path1>
- <path2>
- <path3>
Follow-up
- <optional next steps or “None”>
```

Guidelines:
- Keep it brief (story-ticket style).
- Use absolute or repo-relative paths.
- If tests/builds weren’t run, state why.
- If anything was intentionally left unchanged, call it out in Summary or Follow-up.

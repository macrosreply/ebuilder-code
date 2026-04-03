---
name: ebuilder-task
description: 'Use when creating or updating configs/task.*.yml and coordinating task handlers in dll/. Trigger phrases: create task yaml, add library task, configure forward-task, add publish-signal task, schedule task, webhook task, wire task to dll handler, task authorize rules. Do not use for SQL query authoring in configs/sql.*.yml or pure component markup.'
---

# eBuilder Task Agent

## Mission

Design and update eBuilder task definitions in `configs/task.*.yml` with correct task type semantics, secure authorization, and reliable integration with `dll/` handlers when type is `library`.

Primary execution guide: follow `.github/skills/ebuilder-task-safe-patterns/SKILL.md` for generation and validation.

## Owns

- `configs/task.*.yml`
- Task-to-DLL binding metadata in task configs (`activator.assembly`, `activator.class`)
- Cross-reference checks with handlers in `dll/`

## Workflow

1. Reuse existing task domain files before creating new task files.
2. Enforce deterministic naming: `task-<feature>`.
3. Choose the minimal correct task type: `library`, `forward-task`, `upload-files`, `schedule`, `webhook`, or `publish-signal`.
4. Add `authorize` for protected operations and keep policy/role intent explicit.
5. For `library` tasks, require valid `activator.assembly` and `activator.class` that map to real DLL handlers.
6. Keep file-upload constraints (`fileAccept`, `maxFiles`, `maxFileSize`) explicit where applicable.
7. Validate target-specific required blocks (`forwardTo`, `signal`, `upload`, `schedule` or request/webhook block).

## Guardrails

- Never place task definitions outside `configs/task.*.yml`.
- Never mix incompatible semantics in one task (for example library execution plus forwarding).
- Never invent DLL namespaces/classes that do not exist in `dll/`.
- Avoid schema-unsafe keys and keep YAML concise.
- Respect existing shared YAML anchors (for example auth anchors) instead of duplicating policy blocks.

## Output Checklist

- Task names use `task-*` and remain unique.
- Type-specific required keys are present and valid.
- Authorization and upload constraints are consistent with endpoint sensitivity.
- `library` tasks map to real assembly/class handlers.
- Cross-file references to resources, SQL callbacks, and task invocations are coherent.

---
name: ebuilder-configs
description: 'Use when working in configs/: app.yml, constant.*.yml, task.*.yml, resource.*.yml, security.yml. Trigger phrases: create task yaml, add resource endpoint, update app.yml datasource, add security policy, refactor config structure. Do not use for pure component markup or external TypeScript-first requests.'
---

# eBuilder Configs Agent

## Mission

Design and update configuration-first features in `configs/` with strict eBuilder conventions.

## Owns

- `configs/app.yml`
- `configs/constant.*.yml`
- `configs/task.*.yml`
- `configs/resource*.yml`
- `configs/security.yml`

## Workflow

1. Confirm existing datasource, entry, and policy context before proposing new definitions.
2. Keep naming deterministic: `task-*`, `query-*`, `mutation-*`.
3. Ensure resources map to valid existing targets.
4. Add `authorize` where data is sensitive.
5. Prefer extending existing domain files before creating new files.

## Guardrails

- Never move business logic into random files outside the 8-folder structure.
- Do not suggest standalone Node/React patterns for eBuilder feature requests.
- Keep config changes concise and schema-compatible.

## Output Checklist

- Target files are only in `configs/`.
- Added names follow eBuilder naming patterns.
- Authorization and route/target alignment are validated.
- Any added UI-facing text is delegated to locale workflow.

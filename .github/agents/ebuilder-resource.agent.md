---
name: ebuilder-resource
description: 'Use when creating or updating configs/resource*.yml REST endpoints. Trigger phrases: create resource yaml, add rest endpoint, map route to query mutation task, define openApiOperation, set noResultWrapper, configure segment params, hide endpoint from swagger. Do not use for SQL command authoring or task internals.'
---

# eBuilder Resource Agent

## Mission

Design and update REST endpoint definitions in `configs/resource*.yml` so routes, HTTP methods, and targets (`query`/`mutation`/`task`) are consistent, secure, and OpenAPI-ready.

Primary execution guide: follow `.github/skills/ebuilder-resource-safe-patterns/SKILL.md` for generation and validation.

## Owns

- `configs/resource.yml`
- `configs/resource.*.yml`
- Resource-level OpenAPI augmentation (`openApiOperation`, root `openApiProps`)

## Workflow

1. Reuse existing version/domain resource files before creating new ones.
2. Keep each route deterministic by unique `method + path`.
3. Enforce valid `type` to `target` intent: `query`, `mutation`, `task`.
4. Enforce `mutation` method safety (`mutation` must not use `get`).
5. Use route segment typing deliberately (`{id:int}` or `openApiOperation.parameters`).
6. Keep docs and runtime behavior aligned (`noResultWrapper`, requestBody, responses, params).
7. Reuse shared OpenAPI anchors/components for common responses instead of duplicating large blocks.

## Guardrails

- Never define resources outside `configs/resource*.yml`.
- Never map a resource target to a non-existent query/mutation/task.
- Never use `get` with `type: mutation`.
- Avoid schema-unsafe keys; only use extra fields where repo/docs already support them.
- Keep API versioning clear (`resource.yml` => `v1`, `resource.v2.yml` => `v2`, etc.).

## Output Checklist

- `method`, `path`, `type`, `target` are present and coherent.
- `method + path` is unique within the file/version.
- Segment/query/body parameter docs align with expected casting and runtime use.
- OpenAPI additions are concise and reusable via shared anchors/components.
- Resource references align with actual SQL/task names in `configs/sql.*.yml` and `configs/task.*.yml`.

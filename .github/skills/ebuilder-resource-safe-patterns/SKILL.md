---
name: ebuilder-resource-safe-patterns
description: 'Generation playbook for configs/resource*.yml in eBuilder. USE FOR: define REST endpoints, map method/path/type/target, configure noResultWrapper and segment params, add OpenAPI operation docs, handle API versioning, and enforce target/method safety. DO NOT USE FOR: SQL statement authoring in configs/sql.*.yml or task handler implementation in dll/.'
---

# eBuilder Resource Generation Playbook

## Purpose

Use this skill when generating or reviewing `configs/resource*.yml` so output is:

- aligned with `ebuilder-docs/RestfullAPIs.md`
- compatible with `ebuilder.configs.resource.schema.json`
- consistent with this repository's current resource conventions

## Scope

- File targets: `configs/resource.yml`, `configs/resource.<version>.yml`
- Root keys: `resource`, optional `openApiProps`
- Resource item required core: `method`, `path`, `type`, `target`

## Repo Grounded Conventions

Patterns already used in this repository and should be preserved:

- Shared common HTTP response anchors (for example `.commonHttpErrors: &commonHttpErrors`).
- `resource.yml` (v1) often uses optional `key` for endpoint identity.
- `resource.v2.yml` uses concise endpoint entries plus shared `openApiProps.components` refs.
- `noResultWrapper: true` is used for list/detail query APIs that return raw array/object payloads.
- `openApiOperation` includes tags/summary/description/parameters/requestBody/responses for Swagger quality.

## API Versioning

- `configs/resource.yml` is version `v1` by default.
- `configs/resource.v2.yml` maps to `/api/v2/resource/...`.
- Prefer extending an existing version file when API compatibility allows.
- Create a new version file only for intentional breaking or contract-separated APIs.

## Resource Properties (Schema + Docs)

### Root Object

| Property       | Type   | Required | Notes                                          |
| -------------- | ------ | -------- | ---------------------------------------------- |
| `resource`     | array  | Yes      | Endpoint definitions.                          |
| `openApiProps` | object | No       | Extra OpenAPI root fragments (except `paths`). |

### Resource Item Core Properties

| Property           | Type   | Required | Notes                                                       |
| ------------------ | ------ | -------- | ----------------------------------------------------------- |
| `key`              | string | No       | Repo pattern in `resource.yml`; optional stable identifier. |
| `method`           | enum   | Yes      | `get`, `post`, `put`, `patch`, `delete`.                    |
| `path`             | string | Yes      | Supports path params like `{id}` or typed `{id:int}`.       |
| `type`             | enum   | Yes      | `query`, `mutation`, `task`.                                |
| `target`           | string | Yes      | Target name in SQL/task configs.                            |
| `openApiOperation` | object | No       | OpenAPI operation object for endpoint docs.                 |

### Docs-Supported Extended Properties (Use Carefully)

These properties are described in `RestfullAPIs.md` and used in this repo/runtime, but may not exist in the minimal schema JSON:

| Property             | Type   | Required | Notes                                                      |
| -------------------- | ------ | -------- | ---------------------------------------------------------- |
| `noResultWrapper`    | bool   | No       | Skip `result/results` wrapper in response body.            |
| `noEBLanguageHeader` | bool   | No       | Do not auto-add `x-eb-language` header in generated docs.  |
| `segmentParams`      | object | No       | Explicit segment param mapping/casting to avoid conflicts. |
| `hideFromOpenApi`    | bool   | No       | Hide endpoint from Swagger output.                         |
| `category`           | string | No       | Grouping label used in docs/examples.                      |

Guardrail: when strict schema checks are enabled, prefer schema-listed keys unless repository/runtime confirms support for extended keys.

### Complete Resource Item Props Table

Use this as the single reference for all properties a single `resource` entry can contain.

| Property             | Type    | Required | Allowed values / shape                  | Source status            | Notes                                                                |
| -------------------- | ------- | -------- | --------------------------------------- | ------------------------ | -------------------------------------------------------------------- |
| `key`                | string  | No       | any string                              | schema                   | Optional endpoint identifier, used in current repo (`resource.yml`). |
| `method`             | string  | Yes      | `get`, `post`, `put`, `patch`, `delete` | schema                   | HTTP method of endpoint.                                             |
| `path`               | string  | Yes      | URL path template                       | schema + docs            | Supports `{param}` and typed style in docs such as `{param:int}`.    |
| `type`               | string  | Yes      | `query`, `mutation`, `task`             | schema                   | Target eBuilder service type.                                        |
| `target`             | string  | Yes      | target name string                      | schema                   | Must match existing `query-*`, `mutation-*`, or `task-*`.            |
| `openApiOperation`   | object  | No       | OpenAPI Operation Object                | schema + docs            | Adds tags, parameters, requestBody, responses, etc.                  |
| `noResultWrapper`    | boolean | No       | `true` / `false`                        | docs/runtime extension   | Skip `result/results` response wrapper.                              |
| `noEBLanguageHeader` | boolean | No       | `true` / `false`                        | docs/runtime extension   | Prevent auto `x-eb-language` header in generated OpenAPI docs.       |
| `segmentParams`      | object  | No       | map of segment param definitions        | docs/runtime extension   | Explicit segment mapping/casting to avoid path-vs-body conflicts.    |
| `hideFromOpenApi`    | boolean | No       | `true` / `false`                        | docs/runtime extension   | Hide endpoint from Swagger/OpenAPI output.                           |
| `category`           | string  | No       | any string                              | docs example/runtime use | Grouping label shown in docs/examples.                               |

Schema caveat:

- In `ebuilder.configs.resource.schema.json`, resource item has `additionalProperties: false` and only schema-listed keys are allowed.
- Extended keys from docs (`noResultWrapper`, `segmentParams`, etc.) should be used only when runtime compatibility is confirmed for the target app/engine.

## Method/Type Compatibility Rules

- `type: mutation` must not use `method: get`.
- `type: query` is typically `get`, but non-GET is allowed only when contract intentionally requires body/form behavior.
- `type: task` may use any supported method depending on workflow semantics.
- Keep `method + path` unique in the same version file.

## Target Alignment Rules

Before adding/updating a resource:

1. If `type: query`, `target` must match an existing `query-*` in `configs/sql.*.yml`.
2. If `type: mutation`, `target` must match an existing `mutation-*` in `configs/sql.*.yml`.
3. If `type: task`, `target` must match an existing `task-*` in `configs/task.*.yml`.
4. Ensure target authorization intent is appropriate in underlying SQL/task config.

## Parameter Casting Rules

eBuilder can cast parameters from either route template or OpenAPI parameter schema:

- Path template typed style: `{userId:int}`, `{amount:decimal}`, `{isActive:bool}`.
- OpenAPI parameter schema (`openApiOperation.parameters[].schema.type`) also drives casting for:
  - `in: path`
  - `in: query`
- For query arrays, use OpenAPI `style: form` with `explode: true|false` explicitly.

## Request Shape Rules

- `get`: use path/query params only.
- `post|put|patch|delete`: body can be JSON (`application/json`) or multipart `FormData` (`parameters` field JSON string).
- File upload through resource must use multipart and `files` field.

## OpenAPI Rules

- Keep endpoint-level docs in `openApiOperation`.
- Keep reusable schemas/responses/security in root `openApiProps`.
- Reuse shared anchors or `$ref` components for common errors.
- Prefer concise response schema references over duplicated inline schemas.

## Generation Order

1. Choose target version file (`resource.yml` or `resource.<version>.yml`).
2. Add or update resource item with `method`, `path`, `type`, `target`.
3. Validate method/type compatibility and target existence.
4. Add optional behavior flags (`noResultWrapper`, etc.) only when needed.
5. Add `openApiOperation` for request and response contracts.
6. Add/update shared `openApiProps.components` entries when reuse is beneficial.
7. Recheck route uniqueness and YAML validity.

## YAML Blueprint

```yaml
.commonHttpErrors: &commonHttpErrors
  400:
    $ref: '#/components/responses/400_bad_request'

resource:
  - method: get
    path: /teamInboxes/{inboxId:int}
    type: query
    target: query-api-team-inboxes-by-id
    noResultWrapper: true
    openApiOperation:
      tags: [Inboxes]
      parameters:
        - name: inboxId
          in: path
          required: true
          schema:
            type: integer
      responses:
        200:
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/team_inbox_details'
        <<: *commonHttpErrors

openApiProps:
  components:
    schemas:
      team_inbox_details:
        type: object
```

## Validation Checklist

- `method`, `path`, `type`, `target` are present per resource item.
- No `get` endpoint targets `mutation`.
- Every resource target exists in SQL/task configs and naming is stable.
- Path/query param type casting is documented where needed.
- OpenAPI docs are reusable and not bloated with duplicated schema blocks.
- Extended properties are used only when project/runtime compatibility is clear.

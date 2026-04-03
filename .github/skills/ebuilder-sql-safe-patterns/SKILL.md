---
name: ebuilder-sql-safe-patterns
description: 'Generation playbook for configs/sql.*.yml in eBuilder. USE FOR: generate query-* and mutation-* YAML, design sqlStep chains, apply schema-safe keys, choose responseType, configure catchErrors, placeholders, engine-specific commands, and parameter binding. DO NOT USE FOR: standalone DB scripts outside eBuilder config flow.'
---

# eBuilder SQL Generation Playbook

## Purpose

Use this skill when generating or reviewing `configs/sql.*.yml` so output is:

- valid against `ebuilder.configs.sql.schema.json`
- aligned with eBuilder SQL behavior in `SQL.md`
- safe for user input handling and orchestration

## Scope

- File target: `configs/sql.*.yml`
- Root keys allowed: `query`, `mutation`
- Query names must match: `query-[a-zA-Z0-9-]+`
- Mutation names must match: `mutation-[a-zA-Z0-9-]+`

## eBuilder SQL Symbols (`$EB_*`)

These symbols are critical for portability across database engines. Prefer them over engine-specific raw SQL where possible.

| Symbol                                          | Purpose                                                                  |
| ----------------------------------------------- | ------------------------------------------------------------------------ |
| `$EB_CONSTANT(path)`                            | Read value from constants at runtime.                                    |
| `$EB_CONTEXT(path)`                             | Read value from request/user/runtime context.                            |
| `$EB_PLACEHOLDER(name)`                         | Inject conditional or engine-specific SQL fragments from `placeholders`. |
| `$EB_EXEC ...`                                  | Execute stored procedure/function pattern in portable form.              |
| `$EB_IN_ARRAY_ARG(col, ${arr})`                 | Portable `IN (...)` handling for array args.                             |
| `$EB_DATETIME_NOW`                              | Current datetime across DB engines.                                      |
| `$EB_DATETIME_ADD(exp, n, type)`                | Portable datetime arithmetic.                                            |
| `$EB_GET_DATE(exp)`                             | Date extraction function abstraction.                                    |
| `$EB_DATETIME_TO_ISO_STRING(exp)`               | DateTime to ISO text conversion.                                         |
| `$EB_IF_NULL(a, b)` / `$EB_COALESCE(...)`       | Null fallback logic.                                                     |
| `$EB_TO_LOWER(exp)` / `$EB_TO_UPPER(exp)`       | Case conversion abstraction.                                             |
| `$EB_STR_CONCAT` / `$EB_STRING_CONCAT(...)`     | String concatenation abstraction (`$EB_STRING_CONCAT` preferred).        |
| `$EB_JSON_ARRAY_AGG(col ...)`                   | Portable JSON array aggregation.                                         |
| `$EB_FORWARD_FILTER(field, expr)`               | Bridge REST query filters to SQL predicates.                             |
| `$EB_IF_DEFINED(...)` / `$EB_IF_UNDEFINED(...)` | Conditional SQL fragment inclusion by param presence.                    |
| `$EB_IS_EMPTY_ARRAY(...)`                       | Conditional SQL fragment inclusion for empty arrays.                     |
| `$EB_USER_ID` / `$EB_USER_LANG`                 | Current user metadata abstraction.                                       |
| `$EB_SEQ_NEXT(seq)`                             | Portable sequence/next-id expression.                                    |

Rules:

- Prefer `$EB_STRING_CONCAT(...)` over `$EB_STR_CONCAT` for best cross-DB compatibility.
- Use `$EB_PLACEHOLDER(...)` when SQL text differs by `postgresql`/`oracle`/`mssql`/`mysql`.
- Use `$EB_CONSTANT(...)` and `$EB_CONTEXT(...)` instead of hardcoding deployment/environment values.

## Generation Order

1. Choose `query` or `mutation`.
2. Define `inputParams` with valid parameter types and rules.
3. Set `datasource` and `authorize` when needed.
4. Build SQL with `command` (or step chain for mutation complexity).
5. Add `save` strategy and `responseType`/`response`.
6. Add `checksBefore` or `checksAfter` for guard conditions.
7. Add `catchErrors` mappings for predictable HTTP responses.
8. Validate key names and additional property restrictions.

## Query Blueprint

```yaml
query:
  query-example:
    datasource: main
    authorize:
      policy: eClient
    inputParams:
      itemId:
        type: int
        rules:
          - required: true
    responseType: singleOrNull
    command: |
      SELECT id_item AS "itemId", title AS "title"
      FROM ec_item
      WHERE id_item = ${itemId}
```

Allowed query response types:

- `list`
- `single`
- `singleOrNull`
- `first`
- `firstOrNull`

## Mutation Blueprint

```yaml
mutation:
  mutation-update-example:
    datasource: main
    authorize:
      policy: eClient
    inputParams:
      itemId:
        type: int
        rules:
          - required: true
      title:
        type: string
        rules:
          - required: true
    transactionIsolationLevel: read-committed
    steps:
      - key: updateItem
        command: |
          UPDATE ec_item
          SET title = ${title}
          WHERE id_item = ${itemId}
      - key: result
        command: |
          SELECT id_item AS "itemId", title AS "title"
          FROM ec_item
          WHERE id_item = ${itemId}
        save: first-row-as-object
    response:
      statusCode: 200
      body:
        item: ${result}
```

## SQL Step Rules

Each step must include `key`. Other common properties:

- `command`
- `save`
- `checksBefore`, `checksAfter`
- `catchErrors`
- `stepParams`
- `task` and `taskParams`
- `query` and `queryParams`
- `mutation` and `mutationParams`
- `placeholders`
- `defaultResult`
- `omitResult`

Supported `save` values:

- `scalar`
- `last-insert-id`
- `first-row`
- `first-row-as-object`
- `all-rows`
- `column-values`
- `output-params`

When required by save mode:

- `last-insert-id` should include `extra.identityCol`
- `column-values` should include `extra.column`

## Input and Type Constraints

Allowed `inputParams` types:

- `int`, `decimal`, `number`, `string`, `bool`, `dateTime`, `datetime`, `array`, `object`, `uuid`

Allowed output/property types in `propType`:

- `string`, `bool`, `int`, `double`, `long`, `decimal`, `json`

## Engine-Aware SQL

For `command` and `placeholders`, use either:

- single SQL string
- engine-target object with keys such as `default`, `postgresql`, `oracle`, `mssql`, `mysql`

Use placeholders when SQL differs by engine but query intent is identical.

## Configuration Properties (Complete Tables)

### Root Object

| Property   | Type   | Required | Notes                                       |
| ---------- | ------ | -------- | ------------------------------------------- |
| `query`    | object | No       | Keys must match `^query-[a-zA-Z0-9-]+$`.    |
| `mutation` | object | No       | Keys must match `^mutation-[a-zA-Z0-9-]+$`. |

### Query Object Properties

| Property             | Type                    | Required | Notes                                                                              |
| -------------------- | ----------------------- | -------- | ---------------------------------------------------------------------------------- |
| `inputParams`        | object                  | No       | Parameter schema for request input.                                                |
| `datasource`         | string                  | No       | Target datasource key.                                                             |
| `authorize`          | object or array         | No       | Security policy/roles/permissions.                                                 |
| `responseType`       | enum                    | No       | `list`, `single`, `singleOrNull`, `first`, `firstOrNull`.                          |
| `responseTypeErrors` | object                  | No       | Overrides for `notFound`, `moreThanOne`.                                           |
| `propType`           | object                  | No       | Field type mapping (`string`, `bool`, `int`, `double`, `long`, `decimal`, `json`). |
| `hideProps`          | object                  | No       | Hide or alias output properties.                                                   |
| `customProps`        | object                  | No       | Computed props via `task` or `query`.                                              |
| `pre`                | array of `sqlStep`      | No       | Steps before main query command.                                                   |
| `baseCommand`        | string                  | No       | Reusable base SQL text.                                                            |
| `placeholders`       | object                  | No       | Named SQL fragments for `$EB_PLACEHOLDER(...)`.                                    |
| `command`            | string or engine object | No       | Main SQL statement.                                                                |
| `on`                 | object                  | No       | `success` and `error` callbacks.                                                   |

### Mutation Object Properties

| Property                    | Type                    | Required | Notes                                                           |
| --------------------------- | ----------------------- | -------- | --------------------------------------------------------------- |
| `inputParams`               | object                  | No       | Parameter schema for request input.                             |
| `datasource`                | string                  | No       | Target datasource key.                                          |
| `authorize`                 | object or array         | No       | Security policy/roles/permissions.                              |
| `transactionIsolationLevel` | enum or engine object   | No       | `default`, `read-committed`, `repeatable-read`, `serializable`. |
| `steps`                     | array of `sqlStep`      | No       | Ordered mutation orchestration.                                 |
| `response`                  | object                  | No       | Custom `statusCode`, `message`, `body`.                         |
| `command`                   | string or engine object | No       | Direct mutation command (simple cases).                         |
| `placeholders`              | object                  | No       | Named SQL fragments for `$EB_PLACEHOLDER(...)`.                 |
| `on`                        | object                  | No       | `success` and `error` callbacks.                                |

### SQL Step (`sqlStep`) Properties

| Property         | Type                    | Required | Notes                                                                                                         |
| ---------------- | ----------------------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| `key`            | string                  | Yes      | Unique step id in chain.                                                                                      |
| `log`            | object                  | No       | Step entry logging definition.                                                                                |
| `datasource`     | string                  | No       | Optional datasource override.                                                                                 |
| `jsExpression`   | string                  | No       | Compute result via JS expression.                                                                             |
| `placeholders`   | object                  | No       | Per-step placeholder dictionary.                                                                              |
| `command`        | string or engine object | No       | SQL command for step.                                                                                         |
| `propType`       | object                  | No       | Output typing map.                                                                                            |
| `emitResult`     | boolean                 | No       | Emit step result in response/event context.                                                                   |
| `save`           | enum                    | No       | `scalar`, `last-insert-id`, `first-row`, `first-row-as-object`, `all-rows`, `column-values`, `output-params`. |
| `extra`          | object                  | No       | Save-mode options (`identityCol`, and `column` in docs usage).                                                |
| `checks`         | array                   | No       | Deprecated check property.                                                                                    |
| `checksBefore`   | array                   | No       | Guard checks before execution.                                                                                |
| `checksAfter`    | array                   | No       | Guard checks after execution.                                                                                 |
| `catchErrors`    | object                  | No       | Error-code to response mapping.                                                                               |
| `stepParams`     | any                     | No       | Custom step input binding.                                                                                    |
| `subStepsParams` | any                     | No       | Param projection for sub steps.                                                                               |
| `subSteps`       | array of `sqlStep`      | No       | Nested step chain.                                                                                            |
| `params`         | array                   | No       | Parameter defaults by name.                                                                                   |
| `defaultResult`  | any                     | No       | Result when checks fail and no throw path.                                                                    |
| `omitResult`     | boolean                 | No       | Exclude step output from downstream context.                                                                  |
| `task`           | string                  | No       | Call eBuilder task.                                                                                           |
| `taskParams`     | any                     | No       | Params for `task`.                                                                                            |
| `mutation`       | string                  | No       | Call eBuilder mutation.                                                                                       |
| `mutationParams` | any                     | No       | Params for `mutation`.                                                                                        |
| `query`          | string                  | No       | Call eBuilder query.                                                                                          |
| `queryParams`    | object                  | No       | Query execution options.                                                                                      |

### Check Object (`checksBefore` / `checksAfter` item)

| Property             | Type    | Required | Notes                                                       |
| -------------------- | ------- | -------- | ----------------------------------------------------------- |
| `if`                 | string  | No       | Preferred conditional expression (JS expression string).    |
| `param`              | any     | No       | Deprecated comparison input.                                |
| `op`                 | enum    | No       | `equal`, `notEqual`, `in`, `notIn`, `defined`, `undefined`. |
| `value`              | any     | No       | Deprecated comparison value.                                |
| `elseThrowErrorCode` | number  | No       | Error code when check fails.                                |
| `throwErrorCode`     | number  | No       | Deprecated.                                                 |
| `throw`              | string  | No       | Deprecated.                                                 |
| `silentThrow`        | boolean | No       | Deprecated.                                                 |
| `log`                | object  | No       | Log when condition is true.                                 |
| `elseLog`            | object  | No       | Log when condition is false.                                |

### Query Params (`sqlStep.queryParams`)

| Property      | Type            | Required | Notes                                              |
| ------------- | --------------- | -------- | -------------------------------------------------- |
| `inputParams` | any             | No       | Query input payload.                               |
| `take`        | integer         | No       | Limit.                                             |
| `skip`        | integer         | No       | Offset.                                            |
| `distinct`    | boolean         | No       | Distinct rows.                                     |
| `columns`     | array of string | No       | Select projection control.                         |
| `orders`      | array           | No       | `{ by, direction }`, direction is `ASC` or `DESC`. |
| `filters`     | array           | No       | Filter objects with schema-defined operators.      |

### Mutation/Query Response Objects

| Object                         | Property     | Type    | Notes                                  |
| ------------------------------ | ------------ | ------- | -------------------------------------- |
| `mutationResponse`             | `statusCode` | integer | HTTP status.                           |
| `mutationResponse`             | `message`    | string  | Error or status message.               |
| `mutationResponse`             | `body`       | object  | Custom response payload.               |
| `mutationResponseWithCallback` | `callback`   | object  | Execute `task` or `mutation` callback. |

### SQL Callback (`on.success[]`, `on.error[]`, `callback`)

| Property                  | Type   | Notes                      |
| ------------------------- | ------ | -------------------------- |
| `task`                    | string | Task to execute.           |
| `mutation`                | string | Mutation to execute.       |
| `params`                  | any    | Callback params.           |
| `checks` / `checksBefore` | array  | Optional guard checks.     |
| `log`                     | object | Optional callback logging. |

### SQL Step Log

| Property  | Type   | Required | Notes                                                            |
| --------- | ------ | -------- | ---------------------------------------------------------------- |
| `level`   | enum   | Yes      | `trace`, `debug`, `information`, `warning`, `error`, `critical`. |
| `message` | string | No       | Log message.                                                     |
| `data`    | object | No       | Structured log payload.                                          |

### Input Param Schema

| Property      | Type   | Notes                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------ |
| `description` | string | Parameter description.                                                                           |
| `type`        | enum   | `int`, `decimal`, `number`, `string`, `bool`, `dateTime`, `datetime`, `array`, `object`, `uuid`. |
| `items`       | object | Nested schema for `array` item type.                                                             |
| `properties`  | object | Nested schema for `object` members.                                                              |
| `rules`       | array  | Includes `required`, `nullable`, `enum`, `enumFromConstant`.                                     |

## Inline JavaScript Patterns (`jsExpression` and `if`)

Use inline JavaScript when SQL alone is not enough for step output shaping or conditional guards.

### `jsExpression` Rules

- Always provide a multi-line function body string.
- Always include explicit `return`.
- Keep logic deterministic and side-effect free.
- Prefer using `${...}`, `$EB_CONTEXT(...)`, and `$EB_CONSTANT(...)` as inputs.

Example:

```yaml
steps:
  - key: buildSummary
    jsExpression: |
      ${{
        const count = Number(${totalCount} ?? 0);
        const isAdmin = Boolean($EB_CONTEXT(user.roles)['admin']);
        const category = $EB_CONSTANT(VKB.ITEM_TYPE_IDX_CASE);

        return {
          count,
          isAdmin,
          category,
          label: isAdmin ? 'admin' : 'user'
        };
      }}
```

### `if` Rules (for checks/placeholders)

- Use `if` instead of deprecated `param/op/value` for new code.
- Keep `if` readable and explicit.
- Combine with `elseThrowErrorCode`, `log`, and `elseLog` when useful.

Example in `checksBefore`:

```yaml
checksBefore:
  - if: ${{ ${itemId} > 0 && $EB_CONTEXT(language).startsWith('en') }}
    elseThrowErrorCode: 1001
    log:
      level: debug
      message: check_passed
    elseLog:
      level: warning
      message: check_failed
```

Example in placeholders:

```yaml
placeholders:
  adminClause:
    if: Boolean($EB_CONTEXT(user.roles)['admin'])
    default: AND is_admin = 1
```

## Safety Rules

- Always bind user-provided values via parameters.
- Never concatenate user input into SQL text.
- Prefer checks with `if` conditions; keep deprecated `param/op/value` usage only for compatibility.
- Keep query/mutation naming explicit and stable.
- Use step keys and saved outputs consistently.
- When using `first-row`, reference columns with `stepKey___columnName`.
- If result payload is large, set `omitResult: true` to avoid unnecessary downstream step params growth.

## Error Handling Pattern

Use `catchErrors` as deterministic mapping from SQL or thrown error code to response:

```yaml
catchErrors:
  20001:
    statusCode: 400
    message: CUSTOM_ERROR
  1:
    statusCode: 409
    body:
      message: conflict
```

## Cross-Reference Rules

- If a step calls `task`, `query`, or `mutation`, ensure target names exist.
- Keep resource mappings in `configs/resource*.yml` aligned with generated query/mutation names.
- Keep authorization policy names aligned with `configs/security.yml`.

## Do Not Generate

- Unknown top-level keys outside `query` and `mutation`.
- Invalid name patterns for query/mutation identifiers.
- Raw SQL interpolation from unsanitized user input.
- Ambiguous step keys that collide with input param names unintentionally.

## Deterministic Checklist

1. Confirm target file is `configs/sql.*.yml` and root keys are valid.
2. Validate name patterns for all query/mutation entries.
3. Verify `inputParams` types and rule blocks are schema-safe.
4. Verify every SQL step has `key`, and `save` options are valid.
5. Check `command` and `placeholders` use engine-target shape correctly.
6. Verify parameter binding for all dynamic values.
7. Validate checks and `catchErrors` structure.
8. Validate response strategy:
   - query uses valid `responseType`
   - mutation uses explicit `response` when needed
9. Validate cross-file references to tasks/resources/security policies.
10. Reject unsupported keys to keep schema compatibility strict.
11. Use `$EB_*` symbols intentionally for portability and avoid engine-specific lock-in.
12. When using `jsExpression`, enforce multi-line body with explicit `return`.

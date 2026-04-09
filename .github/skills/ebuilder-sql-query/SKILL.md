---
name: ebuilder-sql-query
description: 'Generation playbook for query-* definitions in configs/sql.*.yml. USE FOR: generate query-* YAML, design single-command queries, apply responseType, configure output field mapping, authorize queries, and handle null responses. DO NOT USE FOR: mutation authoring (use ebuilder-sql-mutation) or symbol reference (use ebuilder-eb-sql-symbols).'
---

# eBuilder Query Generation Playbook

## Purpose

Use this skill when generating or reviewing **`query-*` definitions** in `configs/sql.*.yml`. Queries are **read-only** operations that:

- execute a single primary command
- return data with a specified `responseType`
- support optional `pre` steps for setup
- map output fields with `propType` and `hideProps`
- enforce authorization and security policies

Query output is **deterministic**, **read-only**, and **side-effect free**.

## Scope

- File target: `configs/sql.*.yml`
- Root key: `query` (not `mutation`)
- Query names must match: `query-[a-zA-Z0-9-]+`
- Each query executes **one primary command** (with optional `pre` steps)
- No transactions or multi-step orchestration (use mutations for that)

## Generation Order

1. Choose `query` root key.
2. Define unique query name: `query-<entity>` or `query-<action>-<entity>`.
3. Define `inputParams` with types and rules.
4. Set `datasource` and `authorize` when needed.
5. Build SQL `command` (use `$EB_*` symbols for portability).
6. Choose `responseType` matching expected result shape.
7. Add `propType` for output field type mapping if needed.
8. Add `hideProps` to alias or hide fields if needed.
9. Add `checksBefore` if validation guards are needed.
10. Add `catchErrors` for predictable HTTP responses.
11. Validate all keys and property restrictions.

## Query Blueprint

```yaml
query:
  query-item-by-id:
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
      SELECT id_item AS "itemId", title AS "title", status AS "status"
      FROM ec_item
      WHERE id_item = ${itemId}
```

## Query Object Properties

| Property             | Type                    | Required | Notes                                                                                   |
| -------------------- | ----------------------- | -------- | --------------------------------------------------------------------------------------- |
| `inputParams`        | object                  | No       | Parameter schema for request input.                                                     |
| `datasource`         | string                  | No       | Target datasource key.                                                                  |
| `authorize`          | object or array         | No       | Security policy/roles/permissions.                                                      |
| `responseType`       | enum                    | Yes\*    | `list`, `single`, `singleOrNull`, `first`, `firstOrNull`. (\*required for most queries) |
| `responseTypeErrors` | object                  | No       | Overrides for `notFound`, `moreThanOne`.                                                |
| `propType`           | object                  | No       | Field type mapping (`string`, `bool`, `int`, `double`, `long`, `decimal`, `json`).      |
| `hideProps`          | object                  | No       | Hide or alias output properties.                                                        |
| `customProps`        | object                  | No       | Computed props via `task` or `query`.                                                   |
| `pre`                | array of `sqlStep`      | No       | Steps before main query command (setup, validation, etc).                               |
| `baseCommand`        | string                  | No       | Reusable base SQL text.                                                                 |
| `placeholders`       | object                  | No       | Named SQL fragments for `$EB_PLACEHOLDER(...)`.                                         |
| `command`            | string or engine object | Yes      | Main SQL statement (single string or engine-target object).                             |
| `checksBefore`       | array                   | No       | Guard checks before query execution.                                                    |
| `checksAfter`        | array                   | No       | Guard checks after query execution.                                                     |
| `catchErrors`        | object                  | No       | Error-code to response mapping.                                                         |
| `on`                 | object                  | No       | `success` and `error` callbacks.                                                        |

## Response Types

Choose the response type that matches your result semantics:

| Type           | Result        | Error Conditions               | Use Case                                     |
| -------------- | ------------- | ------------------------------ | -------------------------------------------- |
| `list`         | Array         | None (empty array on no match) | Multiple rows expected (pagination results). |
| `single`       | Object        | `notFound`, `moreThanOne`      | Exactly one row required.                    |
| `singleOrNull` | Object ∣ null | None                           | Zero or one row (optional result).           |
| `first`        | Object        | `notFound`                     | First of many rows required.                 |
| `firstOrNull`  | Object ∣ null | None                           | First row if present.                        |

## Input Parameters and Type Constraints

Define parameters with strict types:

```yaml
inputParams:
  itemId:
    type: int
    rules:
      - required: true

  searchTerm:
    type: string
    rules:
      - required: false
      - nullable: true

  tags:
    type: array
    items:
      type: string
    rules:
      - required: false

  filter:
    type: object
    properties:
      status:
        type: string
      createdAfter:
        type: dateTime
    rules:
      - required: false
```

Allowed input parameter types:

- `int`, `decimal`, `number`, `string`, `bool`, `dateTime`, `datetime`, `array`, `object`, `uuid`

Allowed output property types (`propType`):

- `string`, `bool`, `int`, `double`, `long`, `decimal`, `json`

## Output Field Mapping

### Using `propType`

Map output columns to typed fields:

```yaml
query:
  query-items:
    datasource: main
    responseType: list
    command: |
      SELECT id, title, price, is_active, created_at
      FROM items
    propType:
      price: decimal
      is_active: bool
      created_at: string
```

### Using `hideProps`

Hide sensitive fields or alias columns:

```yaml
query:
  query-user-profile:
    datasource: main
    responseType: singleOrNull
    command: |
      SELECT id, email, password_hash, first_name, last_name
      FROM users
      WHERE id = ${userId}
    hideProps:
      password_hash: true # Hide field (do not return)
      email: user_email # Alias: return as 'user_email'
```

### Using `customProps`

Compute additional fields:

```yaml
query:
  query-item-with-total:
    datasource: main
    responseType: single
    command: |
      SELECT id, name, price, quantity
      FROM items
      WHERE id = ${itemId}
    customProps:
      totalValue:
        task: task-calculate-total
        params:
          price: ${price}
          quantity: ${quantity}
```

## Pre-Steps (Setup and Validation)

Use `pre` steps to execute queries or tasks before the main command:

```yaml
query:
  query-item-with-context:
    datasource: main
    responseType: single
    pre:
      - key: checkAccess
        query: query-user-has-permission
        queryParams:
          inputParams:
            userId: $EB_USER_ID
            resource_type: item
        checksBefore:
          - if: ${{ ${checkAccess} === true }}
            elseThrowErrorCode: 403

    command: |
      SELECT id, name, description
      FROM items
      WHERE id = ${itemId}
```

## Authorization

Add security policies to control query access:

```yaml
query:
  query-user-profile:
    datasource: main
    authorize:
      policy: eClient
    # OR authorize by role
    authorize:
      - role: admin
      - role: support
    # OR authorize by object path
    authorize:
      path: security.policies.readOwnProfile

    responseType: singleOrNull
    command: |
      SELECT id, first_name, email
      FROM users
      WHERE id = $EB_USER_ID
```

## Checks and Validation

### `checksBefore` - Guard Before Execution

```yaml
query:
  query-archived-items:
    datasource: main
    responseType: list
    checksBefore:
      - if: ${{ ${userId} > 0 }}
        elseThrowErrorCode: 400
        elseLog:
          level: warning
          message: invalid_user_id

    command: |
      SELECT id, name FROM items
      WHERE owner_id = ${userId} AND archived = true
```

### `checksAfter` - Guard After Execution

```yaml
query:
  query-single-item:
    datasource: main
    responseType: single
    command: |
      SELECT id, name FROM items WHERE id = ${itemId}

    checksAfter:
      - if: ${{ ${id} !== undefined }}
        log:
          level: debug
          message: item_found
          data:
            itemId: ${id}
```

## Error Handling

Use `catchErrors` to map database or application error codes to HTTP responses:

```yaml
query:
  query-item:
    datasource: main
    responseType: singleOrNull
    command: |
      SELECT * FROM items WHERE id = ${itemId}

    catchErrors:
      20001:
        statusCode: 400
        message: INVALID_ITEM_ID
      1:
        statusCode: 500
        body:
          message: database_error
```

## Response Type Error Handling

Use `responseTypeErrors` to customize error responses when `responseType` constraints fail:

```yaml
query:
  query-item:
    datasource: main
    responseType: single
    command: |
      SELECT * FROM items WHERE status = ${status}

    responseTypeErrors:
      notFound:
        statusCode: 404
        message: NO_ITEMS_FOUND
      moreThanOne:
        statusCode: 409
        message: MULTIPLE_RESULTS_AMBIGUOUS
```

## Callbacks

Execute additional tasks or mutations on success or error:

```yaml
query:
  query-item-with-audit:
    datasource: main
    responseType: singleOrNull
    command: |
      SELECT id, name FROM items WHERE id = ${itemId}

    on:
      success:
        - task: task-log-access
          params:
            itemId: ${itemId}
            user: $EB_USER_ID

      error:
        - task: task-log-error
          checks:
            - if: ${{ ${errorCode} === 20001 }}
          params:
            errorCode: ${errorCode}
```

## Inline JavaScript (`if` and `jsExpression`)

### Conditional Checks with `if`

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

### Output Shaping with `jsExpression` (in `pre` steps)

```yaml
pre:
  - key: prepareParams
    jsExpression: |
      ${{
        const limit = Math.min(${pageSize} || 10, 100);
        const offset = (${pageNum} || 1 - 1) * limit;
        
        return { limit, offset };
      }}
```

Rules for `jsExpression`:

- Always provide **multi-line function body** string.
- Always include explicit **`return`** statement.
- Keep logic **deterministic** and **side-effect free**.
- Use `${...}` parameters, `$EB_CONTEXT(...)`, `$EB_CONSTANT(...)` as inputs.

## Safety Rules

- ✅ **Always bind user inputs via parameters** — never concatenate into SQL.
- ✅ **Use `$EB_*` symbols** for cross-database compatibility.
- ✅ **Set `authorize`** when query accesses sensitive data.
- ✅ **Validate `inputParams`** schema and type constraints.
- ✅ **Map output fields** with `propType` to enforce types.
- ✅ **Hide sensitive fields** with `hideProps` (e.g., password hashes).
- ✅ **Use `checksBefore`** for input validation guards.
- ✅ **Use meaningful query names** (`query-<entity>-by-<key>` pattern).

## Naming Conventions

Follow these patterns for clarity and consistency:

```yaml
# ✅ Good naming
query-item-by-id                    # Fetch single item by primary key
query-items-by-status               # Fetch multiple items by status
query-user-profile                  # Fetch user profile
query-items-count                   # Count operation
query-archive-items-by-date         # Complex query with filter
query-top-items                     # Top N results

# ❌ Avoid
query-data                          # Too generic
query-get                           # Ambiguous verb
query-select-from-table             # Implementation detail
```

## Complete Example

```yaml
query:
  query-items-with-paging:
    datasource: main
    authorize:
      policy: eClient

    inputParams:
      status:
        type: string
        rules:
          - required: true
          - enum:
              - active
              - archived
              - pending

      pageNum:
        type: int
        rules:
          - required: false

      pageSize:
        type: int
        rules:
          - required: false

    pre:
      - key: calcPaging
        jsExpression: |
          ${{
            const pageNum = ${pageNum} || 1;
            const pageSize = Math.min(${pageSize} || 10, 100);
            const offset = (pageNum - 1) * pageSize;
            
            return { offset, pageSize };
          }}

    responseType: list

    command: |
      SELECT 
        id, 
        title, 
        status, 
        created_at AS "createdAt",
        updated_at AS "updatedAt"
      FROM items
      WHERE status = ${status}
      ORDER BY created_at DESC
      OFFSET ${calcPaging___offset}
      LIMIT ${calcPaging___pageSize}

    propType:
      createdAt: string
      updatedAt: string

    checksBefore:
      - if: ${{ ${status} !== undefined }}
        elseThrowErrorCode: 400

    catchErrors:
      400:
        statusCode: 400
        message: INVALID_STATUS
```

## Deterministic Checklist

Before finalizing a query:

1. ✅ Query name matches `^query-[a-zA-Z0-9-]+$` pattern.
2. ✅ `datasource` references valid entry in `configs/app.yml`.
3. ✅ All `inputParams` have `type` and `rules` defined.
4. ✅ All user inputs in `command` are parameter-bound (`${paramName}`).
5. ✅ `responseType` is one of: `list`, `single`, `singleOrNull`, `first`, `firstOrNull`.
6. ✅ Output column aliases use `AS "camelCase"` convention.
7. ✅ `propType` keys match output column aliases.
8. ✅ `hideProps` targets only sensitive or internal fields.
9. ✅ `authorize` policy name exists in `configs/security.yml`.
10. ✅ `pre` steps (if any) use valid `query` or `task` names.
11. ✅ `checksBefore`/`checksAfter` use `if` instead of deprecated `param/op/value`.
12. ✅ `catchErrors` maps only expected error codes.
13. ✅ No deprecated keys (`param`, `op`, `value`, `throw`, `silentThrow`).
14. ✅ SQL uses `$EB_*` symbols where available for portability.

## Do Not Generate

- ❌ Mutations or multi-step orchestration (use `ebuilder-sql-mutation`).
- ❌ Unknown top-level keys outside `query`.
- ❌ Invalid query name patterns.
- ❌ Raw SQL interpolation from unsanitized input.
- ❌ `responseType` value other than the five allowed values.
- ❌ Side effects or data modifications in query `command`.
